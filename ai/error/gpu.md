donut项目报显存不足，解决

```
torch.cuda.OutOfMemoryError: CUDA out of memory. Tried to allocate 76.00 MiB (GPU 0; 11.76 GiB total capacity; 5.82 GiB already allocated; 5.74 GiB free; 5.88 GiB allowed; 5.87 GiB reserved in total by PyTorch) If reserved memory is >> allocated memory try setting max_split_size_mb to avoid fragmentation.  See documentation for Memory Management and PYTORCH_CUDA_ALLOC_CONF
```
os.environ['PYTORCH_CUDA_ALLOC_CONF'] = 'max_split_size_mb:500'

实时查看显卡显存
watch -n 0.1 nvidia-smi
