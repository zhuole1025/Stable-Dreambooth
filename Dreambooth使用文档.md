# Dreambooth文档

### 流程

1. 准备图片

   * 图片最好需要提前裁切到512*512像素，如果不进行裁切的话，在代码中也会自动进行裁切，但是效果可能会受到影响。一个很好用的本地[一键裁切工具](https://github.com/Trainraider/training-image-processor)。

   * 用于训练的图片最好来自具有一定的多样性，包含了不同的角度、背景。
   * 图片最好在50张左右，最少需要大于10张的图片，通常20-30张图片就能取得不错的效果。
   * 将训练图片上传至服务器上的文件夹中

2. 代码关键参数
   * `Model_Version`：要训练的模型版本，可选参数： `["1.5", "V2.1-512px", "V2.1-768px"]`
   * `CKPT_Path`：要训练的模型路径，示例： `"./models/anything_v3.ckpt"`
   * `Session_Name`：本次训练的项目名称，用于创建项目文件夹，示例：`test_db_1`
   * `Remove_existing_instance_images`：是否要将项目文件夹中已上传的图片删掉，可选参数：`[True, False]`
   * `IMAGES_FOLDER_OPTIONAL`：上传图片文件夹的路径，示例： `"./pictures"`
   * `TOKEN`：dreambooth要使用的专用词，也就是给训练图片进行重命名的名称，示例：`"testdb1"`
   * `Crop_images`：是否要自动裁切图片，可选参数：`[True, False]`
   * `Crop_size`：如果要自动裁切图片，则将图片裁切到的尺寸，可选参数：`[512, 576, 640, 704, 768, 832, 896, 960, 1024]`
   * `Add_background`：是否给图片增加白色背景，可选参数：`[True, False]`，适用于给透明背景的png图片加背景
   * `Upscale_image`：是否对图片进行超分辨率，可选参数：`[True, False]`，适用于给低分辨率的图片进行上采样
   * `Anime_image`：是否是动漫图片，可选参数：`[True, False]`
   * `Resume_Training`：是否继续训练，可选参数：`[True, False]`，如果对训练结果不满意，则将其改到`True`并重新运行最后单元格即可继续训练
   * `UNet_Training_Steps`：对UNet训练的步数，最重要的训练参数，通常可以设置为上传的图片数量*60，即如果上传10张图片，将其设置为600；如果对训练结果不满意，可以通过上一个参数进行继续训练
   * `UNet_Learning_Rate`：对UNet训练的学习率，可选参数：`[2e-5, 1e-5, 9e-6, 8e-6, 7e-6, 6e-6, 5e-6, 4e-6, 3e-6, 2e-6]`，通常选择默认的2e-5即可
   * `Text_Encoder_Training_Steps`：对Text Encoder的训练步数
     * 如果让模型学习整体的风格，将该值设置较小即可，通常为UNet训练步数的10-15%，如10张图片使用UNet训练650步，则将该值设置为100左右
     * 如果让模型学习特定的物体，如某个人物，则可以将该值设置偏大一些，可以为UNet训练步数的30-50%，如10张图片使用UNet训练650步，则将该值设置为250左右，如果发现训练失败，则可以将该步数减小
     * 注意，该步数只需设置一次即可，当再次恢复训练时应该将该步数设置为0
   * `Text_Encoder_Learning_Rate`：对Text Encoder训练的学习率，可选参数：`["2e-6", "1e-6","8e-7","6e-7","5e-7","4e-7"]`，要选择较小的值防止过拟合，通常选择默认的1e-6即可
   * `Style_Training`：训练是否让模型去学习某种风格，可选参数：`[True, False]`，勾选的话可以让模型更加关注整体的风格而不是过于关注训练图片中的某个物体
   * `Resolution`：训练图片的分辨率，可选参数：`[512, 576, 640, 704, 768, 832, 896, 960, 1024]`
   * `Save_Checkpoint_Every_n_Steps`：是否固定间隔保存模型权重，可选参数：`[True, False]`
   * `Save_Checkpoint_Every`：训练时每隔多少步保存一次模型权重，示例：`500`
   * `Start_saving_from_the_step`：训练时从多少步以后开始保存模型权重，示例：`1000`