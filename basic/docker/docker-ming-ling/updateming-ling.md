# Update命令

```text
docker update -h
Flag shorthand -h has been deprecated, please use --help

Usage:    docker update [OPTIONS] CONTAINER [CONTAINER...]

Update configuration of one or more containers

Options:
      --blkio-weight uint16         Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --cpu-period int              Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int               Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int           Limit the CPU real-time period in microseconds
      --cpu-rt-runtime int          Limit the CPU real-time runtime in microseconds
  -c, --cpu-shares int              CPU shares (relative weight)
      --cpuset-cpus string          CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string          MEMs in which to allow execution (0-3, 0,1)
      --help                        Print usage
      --kernel-memory string        Kernel memory limit
  -m, --memory string               Memory limit
      --memory-reservation string   Memory soft limit
      --memory-swap string          Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --restart string              Restart policy to apply when a container exits
```

## 修改容器的内存大小

```text
docker update -h
docker update -m 2048m --memory-swap -1 gitlab
```

## 重启策略

```text
docker update --restart=always gitlab
```

