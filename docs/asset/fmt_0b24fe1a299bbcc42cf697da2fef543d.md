
```javascript
// 为所有 select 元素创建自定义下拉框
function customizeSelects() {
    var selects = document.querySelectorAll('select:not([data-customized])');

    for (var i = 0; i < selects.length; i++) {
        (function (originalSelect, index) {
            // 标记为已自定义
            if (originalSelect.hasAttribute('data-customized')) {
                return;
            }
            originalSelect.setAttribute('data-customized', 'true');
            originalSelect.setAttribute('data-custom-id', 'custom-select-' + index);

            var originalStyle = window.getComputedStyle(originalSelect);
            // 保存原始select的尺寸和样式信息
            var originalWidth = originalSelect.offsetWidth;
            var originalHeight = originalSelect.offsetHeight;
            var originalPaddingLeft = originalStyle.paddingLeft;
            var originalPaddingRight = originalStyle.paddingRight;
            var originalFontSize = originalStyle.fontSize;
            var originalFontFamily = originalStyle.fontFamily;
            var originalFontWeight = originalStyle.fontWeight;
            var originalColor = originalStyle.color;
            var originalBorder = originalStyle.border;
            var originalBorderRadius = originalStyle.borderRadius;
            var originalBackgroundColor = originalStyle.backgroundColor;
            // 创建自定义下拉列表 - 直接添加到 body 最后，避免被容器的 overflow 属性截断
            var dropdown = document.createElement('div');
            dropdown.className = 'custom-select-dropdown';
            dropdown.setAttribute('data-for-select', 'custom-select-' + index);
            dropdown.style.display = 'none';
            dropdown.style.position = 'absolute'; // 使用绝对定位
            dropdown.style.width = originalSelect.offsetWidth + 'px';
            dropdown.style.overflowY = 'auto';
            dropdown.style.backgroundColor = '#fff';
            dropdown.style.border = '1px solid #ddd';
            dropdown.style.borderRadius = '4px';
            dropdown.style.boxShadow = '0 2px 5px rgba(0,0,0,0.2)';
            dropdown.style.zIndex = '9999'; // 使用较高的 z-index 确保在顶层
            document.body.appendChild(dropdown); // 添加到 body

            // 记录 select 和 dropdown 的关联
            originalSelect._customDropdown = dropdown;
            dropdown._originalSelect = originalSelect;

            // 填充自定义下拉列表选项
            populateDropdown(originalSelect, dropdown);

            // 拦截原生的下拉框行为
            originalSelect.addEventListener('mousedown', function (e) {
                e.preventDefault(); // 阻止原生下拉框
                e.stopPropagation();
            });

            // 使用click事件来显示下拉框
            originalSelect.addEventListener('click', function (e) {
                e.stopPropagation();
                var dropdown = this._customDropdown;

                // 如果下拉框已经显示，则隐藏它
                if (dropdown.style.display === 'block') {
                    dropdown.style.display = 'none';
                    return;
                }

                // 关闭所有其他下拉框
                closeAllDropdowns();

                // 更新下拉列表选项
                populateDropdown(this, dropdown);

                // 计算并设置下拉框的位置
                positionDropdown(this, dropdown);

                // 显示当前下拉列表
                dropdown.style.display = 'block';
            });

            // 当select值改变时
            originalSelect.addEventListener('change', function () {
                // 如果下拉列表可见，更新选中状态
                var dropdown = this._customDropdown;
                if (dropdown.style.display === 'block') {
                    updateSelectedOption(this, dropdown);
                }
            });

            // 当窗口大小改变时重新定位所有可见的下拉框
            window.addEventListener('resize', function () {
                var visibleDropdowns = document.querySelectorAll('.custom-select-dropdown[style*="display: block"]');
                for (var j = 0; j < visibleDropdowns.length; j++) {
                    var select = visibleDropdowns[j]._originalSelect;
                    positionDropdown(select, visibleDropdowns[j]);
                }
            });

            // 当页面滚动时重新定位所有可见的下拉框
            document.addEventListener('scroll', function () {
                var visibleDropdowns = document.querySelectorAll('.custom-select-dropdown[style*="display: block"]');
                for (var j = 0; j < visibleDropdowns.length; j++) {
                    var select = visibleDropdowns[j]._originalSelect;
                    positionDropdown(select, visibleDropdowns[j]);
                }
            }, true); // 使用捕获阶段以捕获所有滚动事件

        })(selects[i], i);
    }

    // 点击页面其他区域时隐藏所有下拉列表
    document.addEventListener('click', closeAllDropdowns);
}

// 定位下拉框，确保其不会被屏幕边缘截断
function positionDropdown(select, dropdown) {
    var rect = select.getBoundingClientRect();
    var scrollTop = window.pageYOffset || document.documentElement.scrollTop;
    var scrollLeft = window.pageXOffset || document.documentElement.scrollLeft;

    // 计算下拉框的位置
    var top = rect.bottom + scrollTop;
    var left = rect.left + scrollLeft;
    // 检查是否有足够的空间向下展开
    var spaceBelow = window.innerHeight - rect.bottom;

    // 检查水平方向是否有足够的空间
    var dropdownWidth = dropdown.offsetWidth;
    var rightEdge = left + dropdownWidth;
    var viewportWidth = window.innerWidth + scrollLeft;

    // 如果会超出右边界，则向左调整
    if (rightEdge > viewportWidth) {
        left = Math.max(scrollLeft, viewportWidth - dropdownWidth);
    }

    // 设置下拉框的位置
    dropdown.style.top = top + 'px';
    dropdown.style.left = left + 'px';
}

// 关闭所有下拉列表
function closeAllDropdowns() {
    var dropdowns = document.querySelectorAll('.custom-select-dropdown');
    for (var i = 0; i < dropdowns.length; i++) {
        dropdowns[i].style.display = 'none';
    }
}

// 填充下拉列表
function populateDropdown(select, dropdown) {
    // 清空下拉列表
    dropdown.innerHTML = '';

    // 获取原始select元素的计算样式
    var selectStyle = window.getComputedStyle(select);

    // 将字体相关样式应用到下拉框本身
    dropdown.style.fontSize = selectStyle.fontSize;
    dropdown.style.fontFamily = selectStyle.fontFamily;
    dropdown.style.fontWeight = selectStyle.fontWeight;
    dropdown.style.lineHeight = selectStyle.lineHeight;
    dropdown.style.color = selectStyle.color;

    // 重新填充选项
    for (var j = 0; j < select.options.length; j++) {
        (function (optionIndex) {
            var option = document.createElement('div');
            option.className = 'custom-select-option';
            option.textContent = select.options[optionIndex].text;
            option.dataset.value = select.options[optionIndex].value;
            option.style.padding = '0px 2px';
            option.style.cursor = 'pointer';

            // 应用与原始select相同的字体样式
            option.style.fontSize = selectStyle.fontSize;
            option.style.fontFamily = selectStyle.fontFamily;
            option.style.fontWeight = selectStyle.fontWeight;
            option.style.lineHeight = selectStyle.lineHeight;

            // 设置悬停效果
            option.onmouseover = function () {
                this.style.backgroundColor = '#f0f0f0';
            };

            option.onmouseout = function () {
                if (optionIndex !== select.selectedIndex) {
                    this.style.backgroundColor = '#fff';
                }
            };

            // 将当前选中项标记为活动状态
            if (optionIndex === select.selectedIndex) {
                option.style.backgroundColor = '#f0f0f0';
            }

            // 设置点击事件
            option.onclick = function (e) {
                e.stopPropagation();
                select.selectedIndex = optionIndex;

                // 触发原始select的change事件
                var event = new Event('change', {bubbles: true});
                select.dispatchEvent(event);

                dropdown.style.display = 'none';
            };

            dropdown.appendChild(option);
        })(j);
    }
}

// 更新下拉列表选中状态
function updateSelectedOption(select, dropdown) {
    var options = dropdown.querySelectorAll('.custom-select-option');
    for (var i = 0; i < options.length; i++) {
        if (i === select.selectedIndex) {
            options[i].style.backgroundColor = '#f0f0f0';
        } else {
            options[i].style.backgroundColor = '#fff';
        }
    }
}

// 清理函数 - 用于当元素从DOM中移除时清理对应的下拉框
function cleanupDropdowns() {
    var allDropdowns = document.querySelectorAll('.custom-select-dropdown');
    for (var i = 0; i < allDropdowns.length; i++) {
        var dropdown = allDropdowns[i];
        var selectId = dropdown.getAttribute('data-for-select');
        var select = document.querySelector('select[data-custom-id="' + selectId + '"]');

        // 如果找不到对应的select元素，则移除下拉框
        if (!select) {
            dropdown.remove();
        }
    }
}

// 初始化自定义下拉框
customizeSelects();

// 定期清理孤立的下拉框
setInterval(cleanupDropdowns, 5000);

// 为动态添加的select元素添加MutationObserver
var observer = new MutationObserver(function (mutations) {
    var needsUpdate = false;

    mutations.forEach(function (mutation) {
        if (mutation.type === 'childList') {
            needsUpdate = true;
        }
    });

    if (needsUpdate) {
        customizeSelects();
    }
});

observer.observe(document.body, {
    childList: true,
    subtree: true
});
```
