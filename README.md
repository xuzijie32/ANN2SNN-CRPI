# ANN-to-SNN Conversion in Continuous Control Reinforcement Learning 
[[`📕 arXiv`](https://arxiv.org/pdf/2601.21778)] [[`🤗 Hugging Face`](https://huggingface.co/Zijie-Xu/ANN-SNN_Continuous_Control)]

Official code and **model** release for the **ICML 2026** paper 👇
### Error Amplification Limits ANN-to-SNN Conversion in Continuous Control
Zijie Xu, Zihan Huang, Yiting Dong, Kang Chen, Wenxuan Liu, Zhaofei Yu

## Setup
The Conda environments can be found in `./environments`. Note that the MuJoCo tasks (Python=3.7) and DMC tasks (Python=3.8) use different environments.

## Well-trained Models
The models are stored in `./MuJoCo/models` for DDPG/TD3/SAC agents trained for 3M steps. The pretrained DrQ-v2 models are provided in this [GitHub Release](https://github.com/xuzijie32/ANN2SNN-CRPI/releases/tag/Models) (`exp_local.zip`), the checkpoints should be located at `DMC/exp_local/<task_name>/snapshot.pt`. You can convert them directly without re-training the ANNs. You can also see these models in [`Hugging Face`](https://huggingface.co/Zijie-Xu/ANN-SNN_Continuous_Control).

You can also train the ANN models on MuJoCo tasks by running:
```
python DDPG.py --env Hopper-v4 --seed 0
python TD3.py --env Hopper-v4 --seed 0
python SAC.py --env Hopper-v4 --seed 0
```
Or train the ANN models on DMC tasks by running:
```
export MUJOCO_GL=egl
python train.py task@_global_=cartpole_swingup
```

## Converting to SNNs with IF Neurons


Experiments on MuJoCo can be run with:
```
python convert.py --policy_name DDPG --env Hopper-v4 --SNN_ts 16 --eval_seed 0
```
The original ANN policy `--policy_name` can be "DDPG", "TD3", or "SAC". The environment `--env` can be "Ant-v4", "HalfCheetah-v4", "Hopper-v4", or "Walker2d-v4". Running this command will output a .npy file of size `[11][10]`, including 11 different values of $\alpha$: $0,0.1,0.2,\cdots,0.9,1$, and 10 trajectory returns. 


Experiments on DMC can be run with:
```
export MUJOCO_GL=egl
python convert.py --env cartpole_swingup --SNN_ts 32 --seed 0
```
The environment `--env` can be "cartpole_swingup", "finger_spin", "reacher_easy", "cheetah_run", "acrobot_swingup", or "quadruped_walk". Running this command will output a .npy file of size `[11]`, including the average returns for 11 different values of $\alpha$: $0,0.1,0.2,\cdots,0.9,1$. 


## Citing This

To cite our paper and/or this repository in publications:

```bibtex
@article{xu2026error,
  title={Error Amplification Limits ANN-to-SNN Conversion in Continuous Control},
  author={Xu, Zijie and Huang, Zihan and Dong, Yiting and Chen, Kang and Liu, Wenxuan and Yu, Zhaofei},
  journal={arXiv preprint arXiv:2601.21778},
  year={2026}
}
```
