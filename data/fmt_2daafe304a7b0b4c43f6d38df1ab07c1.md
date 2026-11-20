
这个脚本实现了完整的玩家控制。玩家使用 WASD 键(或方向键)移动，角色会根据移动方向播放相应的 4 方向动画(`walk_up/down/left/right`)。当玩家靠近 NPC 时，NPC 会调用`set_nearby_npc()`设置自己为可交互对象，玩家按 E 键即可触发交互。交互时会播放音效，并通过`call_group()`通知对话系统开始对话。对话期间，`set_interacting(true)`会禁用玩家移动，对话结束后恢复移动。走路音效会在玩家移动时自动播放，停止时自动停止。

### 15.5.3 NPC 行为与交互

NPC 需要实现三个核心功能：在场景中随机巡逻游走、响应玩家的交互、显示对话气泡。我们使用 `Area2D` 来检测玩家是否靠近 NPC，当玩家进入交互范围时通知玩家，玩家按 E 键即可开始对话。

NPC 场景的结构包括：`CharacterBody2D` 作为根节点;`CollisionShape2D` 定义 NPC 的碰撞形状;`AnimatedSprite2D` 显示 NPC 动画;`InteractionArea`(`Area2D`)检测玩家进入交互范围，下面有 `CollisionShape2D` 定义交互范围;`NameLabel` 显示 NPC 名字;`DialogueLabel` 显示对话气泡。

NPC 脚本`npc.gd`实现了巡逻、交互和对话气泡逻辑：

```gdscript
extends CharacterBody2D

# NPC信息
@export var npc_name: String = "张三"
@export var npc_title: String = "Python工程师"

# NPC外观配置
@export var sprite_frames: SpriteFrames = null  # 自定义精灵帧资源

# NPC移动配置
@export var move_speed: float = 50.0  # 移动速度
@export var wander_enabled: bool = true  # 是否启用巡逻
@export var wander_range: float = 200.0  # 巡逻范围
@export var wander_interval_min: float = 3.0  # 最小巡逻间隔(秒)
@export var wander_interval_max: float = 8.0  # 最大巡逻间隔(秒)

# 当前对话内容(从后端获取)
var current_dialogue: String = ""

# 节点引用
@onready var animated_sprite: AnimatedSprite2D = $AnimatedSprite2D
@onready var interaction_area: Area2D = $InteractionArea
@onready var name_label: Label = $NameLabel
@onready var dialogue_label: Label = $DialogueLabel

# 玩家引用
var player: Node = null

# 巡逻相关变量
var wander_target: Vector2 = Vector2.ZERO  # 巡逻目标位置
var wander_timer: float = 0.0  # 巡逻计时器
var is_wandering: bool = false  # 是否正在巡逻
var is_interacting: bool = false  # 是否正在与玩家交互
var spawn_position: Vector2 = Vector2.ZERO  # 出生位置

func _ready():
    # 添加到npcs组
    add_to_group("npcs")
    # 设置NPC名字
    name_label.text = npc_name

    # 连接交互区域信号
    interaction_area.body_entered.connect(_on_body_entered)
    interaction_area.body_exited.connect(_on_body_exited)

    # 初始化对话标签
    dialogue_label.text = ""
    dialogue_label.visible = false

    # 设置自定义精灵帧(如果有)
    if sprite_frames != null:
        animated_sprite.sprite_frames = sprite_frames

    # 播放默认动画
    if animated_sprite.sprite_frames != null and animated_sprite.sprite_frames.has_animation("idle"):
        animated_sprite.play("idle")

    # 记录出生位置
    spawn_position = global_position

    # 初始化巡逻计时器
    if wander_enabled:
        wander_timer = randf_range(wander_interval_min, wander_interval_max)
        choose_new_wander_target()

func _on_body_entered(body: Node2D):
    """玩家进入交互范围"""
    if body.is_in_group("player"):
        player = body

        if player.has_method("set_nearby_npc"):
            player.set_nearby_npc(self)

func _on_body_exited(body: Node2D):
    """玩家离开交互范围"""
    if body.is_in_group("player"):
        if player != null and player.has_method("set_nearby_npc"):
            player.set_nearby_npc(null)
        player = null

func update_dialogue(dialogue: String):
    """更新NPC对话内容"""
    current_dialogue = dialogue
    dialogue_label.text = dialogue
    dialogue_label.visible = true

    # 10秒后隐藏对话
    await get_tree().create_timer(10.0).timeout
    dialogue_label.visible = false

func _physics_process(delta: float):
    """物理更新 - 处理移动"""
    # 如果正在与玩家交互,停止移动
    if is_interacting:
        velocity = Vector2.ZERO
        move_and_slide()
        # 播放idle动画
        if animated_sprite.sprite_frames != null and animated_sprite.sprite_frames.has_animation("idle"):
            animated_sprite.play("idle")
        return

    # 如果未启用巡逻,不移动
    if not wander_enabled:
        return

    # 更新巡逻计时器
    wander_timer -= delta

    # 如果计时器结束,选择新目标并开始移动
    if wander_timer <= 0:
        choose_new_wander_target()
        wander_timer = randf_range(wander_interval_min, wander_interval_max)

    # 如果正在巡逻,移动到目标
    if is_wandering:
        # 检查是否到达目标
        if global_position.distance_to(wander_target) < 10:
            # 到达目标,停止移动
            is_wandering = false
            velocity = Vector2.ZERO
            move_and_slide()
            # 播放idle动画
            if animated_sprite.sprite_frames != null and animated_sprite.sprite_frames.has_animation("idle"):
                animated_sprite.play("idle")
        else:
            # 继续移动到目标
            var direction = (wander_target - global_position).normalized()
            velocity = direction * move_speed
            move_and_slide()
            # 更新动画
            update_animation(direction)
    else:
        # 停止移动
        velocity = Vector2.ZERO
        move_and_slide()
        # 播放idle动画
        if animated_sprite.sprite_frames != null and animated_sprite.sprite_frames.has_animation("idle"):
            animated_sprite.play("idle")

func choose_new_wander_target():
    """选择新的巡逻目标"""
    # 在出生位置附近随机选择一个点
    var offset = Vector2(
        randf_range(-wander_range, wander_range),
        randf_range(-wander_range, wander_range))
    wander_target = spawn_position + offset
    is_wandering = true

func update_animation(direction: Vector2):
    """更新动画"""
    if animated_sprite.sprite_frames == null:
        return

    if direction.length() > 0:
        # 移动动画
        if abs(direction.x) > abs(direction.y):
            # 左右移动
            if direction.x > 0:
                if animated_sprite.sprite_frames.has_animation("walk_right"):
                    animated_sprite.play("walk_right")
                elif animated_sprite.sprite_frames.has_animation("walk"):
                    animated_sprite.play("walk")
                    animated_sprite.flip_h = false
            else:
                if animated_sprite.sprite_frames.has_animation("walk_left"):
                    animated_sprite.play("walk_left")
                elif animated_sprite.sprite_frames.has_animation("walk"):
                    animated_sprite.play("walk")
                    animated_sprite.flip_h = true
        else:
            # 上下移动
            if direction.y > 0:
                if animated_sprite.sprite_frames.has_animation("walk_down"):
                    animated_sprite.play("walk_down")
                elif animated_sprite.sprite_frames.has_animation("walk"):
                    animated_sprite.play("walk")
            else:
                if animated_sprite.sprite_frames.has_animation("walk_up"):
                    animated_sprite.play("walk_up")
                elif animated_sprite.sprite_frames.has_animation("walk"):
                    animated_sprite.play("walk")
    else:
        # 静止动画
        if animated_sprite.sprite_frames.has_animation("idle"):
            animated_sprite.play("idle")

func set_interacting(interacting: bool):
    """设置交互状态"""
    is_interacting = interacting
```

这个脚本实现了 NPC 的完整行为。NPC 会在出生位置附近的`wander_range`范围内随机巡逻，每隔`wander_interval_min`到`wander_interval_max`秒选择一个新的目标点并移动过去。移动时会播放 4 方向动画(`walk_up/down/left/right`)，到达目标后停止并播放 `idle` 动画。当玩家进入 `InteractionArea` 时，NPC 会调用玩家的`set_nearby_npc(self)`方法，将自己设置为可交互对象。玩家按 E 键后，对话系统会调用 NPC 的`set_interacting(true)`方法，NPC 停止移动。对话结束后调用`set_interacting(false)`，NPC 恢复巡逻。主场景会定时调用`update_dialogue()`方法更新 NPC 的对话气泡，显示 NPC 之间的自主对话内容。

## [15.6 前后端通信实现](https://datawhalechina.github.io/hello-agents/#/./chapter15/%E7%AC%AC%E5%8D%81%E4%BA%94%E7%AB%A0%20%E6%9E%84%E5%BB%BA%E8%B5%9B%E5%8D%9A%E5%B0%8F%E9%95%87?id=_156-%e5%89%8d%e5%90%8e%e7%ab%af%e9%80%9a%e4%bf%a1%e5%ae%9e%e7%8e%b0)

### 15.6.1 API 客户端封装

Godot 前端需要与 FastAPI 后端进行 HTTP 通信。我们创建一个 API 客户端脚本`api_client.gd`，封装所有的 API 调用，并将其设置为 `AutoLoad`(自动加载)单例，让其他脚本可以方便地使用。

API 客户端使用 Godot 的 `HTTPRequest` 节点来发送 HTTP 请求。`HTTPRequest` 是一个异步节点，发送请求后不会阻塞游戏，而是通过信号通知请求完成。这样可以保证游戏的流畅性，即使网络延迟较高也不会卡顿。我们使用信号机制来通知其他脚本 API 响应，而不是使用 `await`，这样可以让多个脚本同时监听同一个 API 响应。
