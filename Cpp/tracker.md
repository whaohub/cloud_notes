## DeepSort工作流程

DeepSORT对每一帧的处理流程如下：
检测器得到bbox → 生成detections → 卡尔曼滤波预测→ 使用匈牙利算法将预测后的tracks和当前帧中的detecions进行匹配（级联匹配和IOU匹配） → 卡尔曼滤波更新

> Frame 0：检测器检测到了3个detections，当前没有任何tracks，将这3个detections初始化为tracks
> Frame 1：检测器又检测到了3个detections，对于Frame 0中的tracks，先进行预测得到新的tracks，然后使用匈牙利算法将新的tracks与detections进行匹配，得到(track, detection)匹配对，最后用每对中的detection更新对应的track



https://juejin.cn/post/7052864298216849439