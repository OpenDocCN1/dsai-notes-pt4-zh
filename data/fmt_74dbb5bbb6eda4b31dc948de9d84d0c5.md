
```python
def speculative_sampling(target_model, draft_model, initial_prompt_seq, target_len, tokenizer, lookahead=4, temperature=1.0, debug=True):
    '''
    Implementation of Algorithm 2 of the paper - Accelerating Large Language Model Decoding 
    with Speculative Sampling (https://arxiv.org/abs/2302.01318)
    '''
    assert initial_prompt_seq.shape[0] == 1, 'Batch size should be 1'

    n = initial_prompt_seq.shape[-1] # 13
    fin_prompt_seq = initial_prompt_seq.detach().clone() # torch.Size([1, 13])

    while n < target_len: # 64 + 1 = 65
        n_orig = n # 13
        N = fin_prompt_seq.shape[-1] # 13
        # OPTForCausalLM opt_model + lmhead (768, 50272)
        # 1 * 4
        draft_outputs, draft_logits = sample_from_draft_model(draft_model, fin_prompt_seq, new_tokens=lookahead, temperature=temperature)
        # torch.Size([1, 17]), torch.Size([1, 4, 50272])
        if debug:
            print(f"Possible continuations: {tokenizer.decode(draft_outputs[0,n_orig:], skip_special_tokens=True)}")
        
        # x + 5
        # OPTForCausalLM opt_model + lmhead (2048, 50272)
        target_logits = target_model(draft_outputs).logits[:, -lookahead-1:, :] 
        # torch.Size([1, 5, 50272])
        
        target_model_distribution = get_distribution(target_logits, temperature)
        draft_model_distribution = get_distribution(draft_logits, temperature)
        accepted_flag = 1 # 初始化接受标志 accepted_flag 为1。
        
        for t in range(lookahead):
            numerator = target_model_distribution[:, t, draft_outputs[0, N+t]]
            denominator = draft_model_distribution[:, t, draft_outputs[0, N+t]]
            ratio = (numerator / denominator) 
            ones_tensor = torch.ones_like(numerator)

            uniform_distribution = torch.rand_like(numerator)

            # Rejection Sampling
            ## Acceptance 
            # 如果随机值小于概率比，则接受草稿模型的生成结果，并更新 fin_prompt_seq 和增加 n。
            # print('uniform_distribution:{}_ratio:{}'.format(uniform_distribution, ratio))
            if (uniform_distribution < torch.min(ones_tensor, ratio)).any():
                fin_prompt_seq = torch.concat([fin_prompt_seq, draft_outputs[:, N+t].unsqueeze(dim=-1)], dim=-1)
                n += 1
                print('{}_lookhead_{}'.format(n, t))
                # 小循环

            ## Rejection
            # 否则，拒绝草稿模型的结果，重新采样目标模型的分布，并更新 fin_prompt_seq，同时将 accepted_flag 设置为0并跳出循环。
            else:
                new_dist = (target_model_distribution[:, t, :] - draft_model_distribution[:, t, :])
                new_dist = torch.max(torch.zeros_like(new_dist), new_dist)
                new_dist = new_dist / new_dist.sum(dim=-1, keepdim=True)
                token_id = torch.multinomial(new_dist, num_samples=1)[0]
                
                fin_prompt_seq = torch.concat([fin_prompt_seq, token_id[None,...]], dim=-1)
                accepted_flag = 0
                break

        if accepted_flag == 1:
            # 如果所有时间步都接受了草稿模型的结果，则从目标模型的最后一步分布中采样一个token，并添加到 fin_prompt_seq 中。
            sample_token = sample(target_logits[:, -1, :], temperature=temperature)
            fin_prompt_seq = torch.concat([fin_prompt_seq, sample_token[None,...]], dim=-1)
        
        if debug:
            print(f"Accepted continuations: {tokenizer.decode(fin_prompt_seq[0,n_orig:], skip_special_tokens=True)}")

        n += 1
        print('{}_target_model'.format(n))

    return fin_prompt_seq
```
