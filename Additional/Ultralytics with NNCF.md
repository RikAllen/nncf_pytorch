# Instructions for use of Ultralytics with NNCF

1. Download the 2017 COCO dataset (2017 Train images, 2017 Val images, 2017 Test images, 2017 Train/Val annotations) from [https://cocodataset.org/#download](https://cocodataset.org/#download) into a folder
2. Into a new directory, clone both Ultralytics ([https://github.com/ultralytics/yolov3](https://github.com/ultralytics/yolov3)) and Retraining&#39;s nncf\_pytorch repo ([https://github.com/RikAllen/nncf\_pytorch](https://github.com/RikAllen/nncf_pytorch)).
3. Yolov3 repository should be reverted to commit _cf652962fdd11b3e3ed164fc21ccb065101927aa_ due to a tensor size error introduced in later commits.
4. Copy the yolov3 code into the nncf\_pytorch folder.
5. Switch to block\_floating\_point branch.
6. Edit Additional/YOLO\_integration.patch to change pathto/coco to your path for coco.
7. Apply patch:
    - git apply --stat Additional/YOLO\_integration.patch - will show you stats about what the patch will do.
    - git apply --check Additional/YOLO\_integration.patch - if you don&#39;t get any errors, then the patch can be applied cleanly, otherwise it&#39;ll indicate where errors are.
    - Make sure any files are committed before running the command below (git add and git commit).
    - git am Additional/YOLO\_integration.patch - applies the patch to files.
8. Setup Conda environment for NNCF and YOLO **\***
9. Install NNCF and set up pythonpath.
10. Set appropriate hw\_config\_subtype in Additional/tinyyolov3\_Ultralytics.json file.
11. To train TinyYoloV3, use this command:
    - python train.py --cfg cfg/yolov3-tiny.cfg --batch-size 28 --epochs 10 --weights weights/yolov3-tiny.pt
12. To test TinyYoloV3 using original tinyyolo weights or weights/NNCF\_best.pt created from the last training run:
    - python test.py --cfg cfg/yolov3-tiny.cfg --batch-size 28 --weights weights/yolov3-tiny.pt (or weights/NNCF\_best.pth) --img-size 416
        1. If you are using yolov3-tiny.pt, set retrained = False in test.py
        2. If you are using weights from retraining, set retrained = True in test.py
        3. If you want to export to an ONNX file, make directory nncf\_pytorch/onnxFiles and set ONNX\_EXPORT = True in test.py. Change model\_name variable in line 77 to suitable name for ONNX.

**\*** Make sure

TORCH\_VERSION = &quot;1.5.0&quot;

TORCHVISION\_VERSION = &quot;0.6.0&quot;

This is due to NNCF&#39;s lack of support for higher versions, using new versions may result in unexpected errors.