## 解包打包
### redmi_12C 13
1. sparse 格式 super.img 解包正常(各分区md5与TK解包一致)
2. erofs 格式 system.img 解包打包正常(md5与TK解包一致)
3. super.img 解包后直接打包super后开机失败, 返回fastboot, 解包时显示有a/b两个分区, 所有打包时选Virtual A/B 模式
4. 分区类型(Virtual A/B, A-Only, A/B)选择:
   1. 新设备/主流: 推荐 Virtual A/B (平衡空间与安全性)
   2. 低端设备/空间受限: 可选 A-Only
   3. 高端/需要物理隔离: 选择 A/B
   4. 判断依据:
      1. extents 分布
         1. _a 分区有 extents 数据 → 物理上分配了空间
         2. _b 分区 extents 为空 → 物理上未分配空间，通过 snapshot 虚拟化
      2. 分区命名
         1. 同时存在 _a 和 _b 后缀的分区
         2. 但只有 _a 在 partition_layout 中显示物理位置
      3. group_table 特点
         1. main_a_a 和 main_a_b 的 maximum_size 相同, 这是 Virtual A/B 的典型特征：逻辑上两个槽位，物理上共享空间

5. 簇名选择(无关重要?)
   1. 簇名默认, 开机.
   2. 簇名选main, 开机.
   3. 簇名选main_a, 开机.
   4. 簇名选qti_dynamic_partitions, 开机.

6. 如果有新增文件, 要在 "设置" -- "Others" -- "Selinux 上下文修补" 选择, 否则可能不起效.