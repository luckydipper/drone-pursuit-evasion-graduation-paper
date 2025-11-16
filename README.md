# Installation method 
```
uv venv --python 3.12.0
source .venv/bin/activate
uv pip install -r requirements.txt
```

# 🚁 Drone Pursuit-Evasion Multi-Agent RL System

![Drone Pursuit-Evasion](https://img.shields.io/badge/Environment-3D_Simulation-green.svg)
![Algorithms](https://img.shields.io/badge/Algorithms-DQN_|_PPO_|_SAC-blue.svg)

# Demo Videos
| Reinforcement Learning PPO Drone Pursuit-Evade | LiDAR Ray Cast in PyBullet Engine |
|:---:|:---:|
| [![PPO Pursuit-Evade Demo](https://img.youtube.com/vi/AHSQWR0wB9c/maxresdefault.jpg)](https://youtu.be/AHSQWR0wB9c) | [![LiDAR Ray Cast Demo](https://img.youtube.com/vi/s36m3Azg3rc/maxresdefault.jpg)](https://youtu.be/s36m3Azg3rc) |
| Click to watch PPO training results | Click to watch LiDAR sensor visualization |

## 🚀 Quick Start

### Installation
```bash
# Clone repository
git clone <repository-url>
cd drone-pursuit-evasion

# Install dependencies
pip install -r requirements.txt

# Verify installation
python -c "import pybullet; print('PyBullet installed successfully')"
```

### Basic Training
```bash
# Train DQN vs DQN (default)
python train_hydra.py

# Train SAC vs SAC (advanced)
python train_hydra.py scenario=sac_vs_sac

# Train PPO vs Random (quick test)
python train_hydra.py scenario=ppo_pursuer_vs_random_evader training.total_timesteps=10000
```

### Environment Complexity Stages
```bash
# Stage 1: Open space (no obstacles) - Easy
python train_hydra.py scenario=sac_vs_sac environment.stage=open

# Stage 2: Single obstacle - Medium
python train_hydra.py scenario=sac_vs_sac environment.stage=single

# Stage 3: Multiple obstacles - Hard (default)
python train_hydra.py scenario=sac_vs_sac environment.stage=multiple
```

### Evaluation
```bash
# Evaluate latest models with GUI
python evaluate_hydra.py

# Evaluate without GUI (headless)
python evaluate_trained_models.py --episodes 10
```

### Visualization
```bash
# Visualize pretrained models with 3D simulation
python visualize_hydra.py

# Visualize specific scenario and stage
python visualize_hydra.py scenario=sac_vs_sac environment.stage=single

# Load specific model weights
python visualize_hydra.py visualization.weights_dir=weights/my_experiment/20241201_143022
```

## 🎯 Features

### **Multi-Algorithm Support**
- **DQN**: Deep Q-Network with experience replay
- **PPO**: Proximal Policy Optimization with GAE
- **SAC**: Soft Actor-Critic with twin critics
- **Special Agents**: Random and Hovering agents for baselines

### **Environment Complexity Stages**
- **Stage 1 (Open)**: Pure pursuit-evasion in open space - ideal for algorithm development
- **Stage 2 (Single)**: Basic obstacle avoidance with one central cylinder
- **Stage 3 (Multiple)**: Complex navigation through grid of obstacles
- **Curriculum Learning**: Progressive training from simple to complex scenarios

## 📊 Available Training Scenarios

### **Algorithm Comparison Matrix**

| Pursuer → <br> Evader ↓ | PPO | DQN | SAC |
|-------------------------|-----|-----|-----|
| **Hovering** | `ppo_pursuer_vs_hovering_evader` | `dqn_pursuer_vs_hovering_evader` | `sac_pursuer_vs_hovering_evader` |
| **Random** | `ppo_pursuer_vs_random_evader` | `dqn_pursuer_vs_random_evader` | `sac_vs_random` |
| **PPO** | *Configure manually* | `dqn_pursuer_vs_ppo_evader` | `sac_pursuer_vs_ppo_evader` |
| **DQN** | `ppo_pursuer_vs_dqn_evader` | `pursuit_evasion` | `sac_pursuer_vs_dqn_evader` |
| **SAC** | *Create new scenario* | *Create new scenario* | `sac_vs_sac` |

### **Quick Training Commands**
```bash
# Basic Algorithm Testing
python train_hydra.py scenario=ppo_pursuer_vs_hovering_evader
python train_hydra.py scenario=dqn_pursuer_vs_hovering_evader  
python train_hydra.py scenario=sac_pursuer_vs_hovering_evader

# Cross-Algorithm Competition
python train_hydra.py scenario=ppo_pursuer_vs_dqn_evader
python train_hydra.py scenario=sac_pursuer_vs_ppo_evader

# Symmetric Training (Both agents learning)
python train_hydra.py scenario=pursuit_evasion  # DQN vs DQN
python train_hydra.py scenario=sac_vs_sac       # SAC vs SAC
```

## 🔧 Configuration System

### **Hydra-Based Configuration**
```bash
# Override hyperparameters
python train_hydra.py agent.learning_rate=0.001 agent.gamma=0.95

# Change training duration  
python train_hydra.py training.total_timesteps=50000

# Enable/disable logging
python train_hydra.py wandb.mode=online
python train_hydra.py wandb.mode=disabled
```

### **Configuration Structure**
```
conf/
├── config.yaml              # Main configuration
├── agent/
│   ├── dqn.yaml             # DQN hyperparameters
│   ├── ppo.yaml             # PPO hyperparameters
│   └── sac.yaml             # SAC hyperparameters
└── scenario/
    ├── pursuit_evasion.yaml      # DQN vs DQN
    ├── sac_vs_sac.yaml          # SAC vs SAC
    ├── ppo_pursuer_vs_dqn_evader.yaml
    └── ... (many more scenarios)
```

## 📈 Performance Monitoring

### **WandB Integration**
- **Real-time Metrics**: Training loss, episode rewards, capture rates
- **Detailed Evaluation**: Episode tables, outcome statistics
- **Hyperparameter Tracking**: Automatic configuration logging
- **Model Checkpointing**: Best and periodic model saving

### **Evaluation Metrics**
- **Success Rates**: Capture, collision, timeout, out-of-bounds
- **Episode Statistics**: Length, rewards, outcome timing
- **Agent Performance**: Individual and comparative analysis

## 📚 Documentation

## 🎮 Usage Examples

### **Algorithm Comparison Study**
```bash
# Train all algorithms against static target
python train_hydra.py scenario=ppo_pursuer_vs_hovering_evader experiment_name=ppo_baseline
python train_hydra.py scenario=dqn_pursuer_vs_hovering_evader experiment_name=dqn_baseline  
python train_hydra.py scenario=sac_pursuer_vs_hovering_evader experiment_name=sac_baseline

# Evaluate and compare
python evaluate_trained_models.py --episodes 50
```

### **Hyperparameter Sweeps**
```bash
# Learning rate comparison
for lr in 0.001 0.0005 0.0001; do
  python train_hydra.py agent.learning_rate=$lr experiment_name=lr_sweep_$lr &
done
```

### **Custom Scenarios**
Create new scenarios in `conf/scenario/` for specific research needs:
```yaml
# conf/scenario/my_custom_scenario.yaml
drones:
  - role: PURSUER
    agent_type: sac
    start_pos: [2.0, 2.0, 1.0]
    action_length: 10.0
    is_training: true
  - role: EVADER
    agent_type: ppo
    start_pos: [-2.0, -2.0, 1.0]
    action_length: 5.0
    is_training: true
```

## 🎓 Curriculum Learning with Environment Stages

### **Progressive Training Strategy**

Instead of using separate environment config files, simply override the `stage` parameter for curriculum learning:

```bash
# 🌌 Stage 1: Start with open space (easiest)
python train_hydra.py scenario=pursuit_evasion environment.stage=open training.total_timesteps=25000 experiment_name=curriculum_stage1

# 🏗️ Stage 2: Add single obstacle (medium difficulty)  
python train_hydra.py scenario=pursuit_evasion environment.stage=single training.total_timesteps=25000 experiment_name=curriculum_stage2

# 🌆 Stage 3: Full complexity with multiple obstacles (hardest)
python train_hydra.py scenario=pursuit_evasion environment.stage=multiple training.total_timesteps=50000 experiment_name=curriculum_stage3
```

### **Stage Parameter Options**
- `environment.stage=open` - No obstacles (pure pursuit-evasion)
- `environment.stage=single` - One central cylinder obstacle
- `environment.stage=multiple` - Grid of obstacles (default)

### **Quick Stage Testing**
```bash
# Test any scenario across all stages quickly
python train_hydra.py scenario=sac_vs_sac environment.stage=open training.total_timesteps=5000
python train_hydra.py scenario=sac_vs_sac environment.stage=single training.total_timesteps=5000  
python train_hydra.py scenario=sac_vs_sac environment.stage=multiple training.total_timesteps=5000
```
