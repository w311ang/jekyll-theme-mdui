---
layout: post
title: 在Hyper-V中启用vGPU
date: 2021-07-05 07:44:44 +0800
categories: technology
tags: hyperv vgpu 
img: 
---
1. 关闭该虚拟机的还原点
2. 在实体机中以管理员身份运行的PowerShell执行`$vm = "你的虚拟机名"`
3. 再复制执行以下

```powershell
Add-VMGpuPartitionAdapter -VMName $vm
Set-VMGpuPartitionAdapter -VMName $vm -MinPartitionVRAM 80000000 -MaxPartitionVRAM 100000000 -OptimalPartitionVRAM 100000000 -MinPartitionEncode 80000000 -MaxPartitionEncode 100000000 -OptimalPartitionEncode 100000000 -MinPartitionDecode 80000000 -MaxPartitionDecode 100000000 -OptimalPartitionDecode 100000000 -MinPartitionCompute 80000000 -MaxPartitionCompute 100000000 -OptimalPartitionCompute 100000000

Set-VM -GuestControlledCacheTypes $true -VMName $vm
Set-VM -LowMemoryMappedIoSpace 1Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 32GB -VMName $vm
```

4. 安装软碟通将实体机中`C:\Windows\System32\DriverStore\`下的`FileRepository`文件夹拖入右上方框中，再点击左上角文件 - 另存为保存为一个ISO镜像
5. 如果你是N卡，还需要将`C:\Windows\System32\nvapi64.dll`一并拖入内
6. 然后将该ISO镜像挂载到该虚拟机的光驱
7. 将`FileRepository`文件夹复制到虚拟机`C:\Windows\System32\HostDriverStore\`
8. 如果你是N卡，也将`nvapi64.dll`复制到`C:\Windows\System32\`
9. 完成上述操作后，重启