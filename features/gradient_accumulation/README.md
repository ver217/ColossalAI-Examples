# Gradient Accumulation

## Prepare Dataset

In the script, we used CIFAR10 dataset provided by the `torchvision` library. The code snippet is shown below:

```python
train_dataset = CIFAR10(
        root=Path(os.environ['DATA']),
        download=True,
        transform=transforms.Compose(
            [
                transforms.RandomCrop(size=32, padding=4),
                transforms.RandomHorizontalFlip(),
                transforms.ToTensor(),
                transforms.Normalize(mean=[0.4914, 0.4822, 0.4465], std=[
                    0.2023, 0.1994, 0.2010]),
            ]
        )
    )
```

Firstly, you need to specify where you want to store your CIFAR10 dataset by setting the environment variable `DATA`. 

```bash
export DATA=/path/to/data

# example
# this will store the data in the current directory
export DATA=$PWD/data
```

The `torchvison` module will download the data automatically for you into the specified directory.


## Verify Gradient Accumulation

To verify gradient accumulation, we can just check the change of parameter values. When gradient accumulation is set, parameters
are only updated in the last step. 

```bash
python -m torch.distributed.launch --nproc_per_node 1 --master_addr localhost --master_port 29500  train_with_engine.py
```

If you have PyTorch version >= 1.10, you can use `torchrun` instead.

```bash
torchrun --standalone --nnodes=1 --nproc_per_node 1 train_with_engine.py
```