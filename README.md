# motion-to-fbx
 - How to convert motion animation obtained using diffusion model to SMPL FBX file for Unity
 - Diffusion 모델을 통해 추출된 모션에니메이션을 SMPL 모델의 FBX 파일로 변환하여 Unity에서 Import하는 방법을 다룹니다.

# Demo

![ezgif-4-41bca8b5b4](https://user-images.githubusercontent.com/18140805/209769258-648ac671-ed50-46b1-a99b-b992e5ed7641.gif)

## 1) Text to Motion
- Input: motion text (input.txt)
- Output: a set of [frame, joints, position] (.npy) 
- Procedure
  - put motion text in input.txt as below:
  ```
  A person jumps up and down.
  ```
  - execute command
  ```
  python gen_motion_script.py --name Comp_v6_KLD01 --text_file input.txt --repeat_time 3 --ext customized --gpu_id 0
  ```
  - generated animation (gif)

    ![a-person-jumps-up-and-down](https://user-images.githubusercontent.com/18140805/209764875-ba34c14d-d229-4f77-9273-417d01eed714.gif)
- Reference
  - [Generating Diverse and Natural 3D Human Motions from Text (CVPR 2022)](https://github.com/EricGuo5513/text-to-motion)

## 2) Motion to SMPL

- Input: a set of frame, joints, position (.npy)
- Output: a set of [frame, smpl_poses] and [frame, smpl_trans] (.pkl)
- Procedure
  - Overwrite fit_seq.py
  - copy .npy file of 22 joints to demo_data folder 
    - Source
      - text-to-motion\eval_results\t2m\Comp_v6_KLD01\customized\animations\C000\XXX.npy
    - Target
      - joints2smpl\demo\demo_data\XXX.pkl
  - execute command
     ```
     python fit_seq.py --files XXX.npy
     ```
- Reference
  - [joints2smpl](https://github.com/wangsen1312/joints2smpl)

## 3) SMPL to FBX
- Input: a set of [frame, smpl_poses] and [frame, smpl_trans] (.pkl)
- Output: SMPL FBX model for Unity (.fbx)
- Procedure
  - copy the reference SMPL fbx file to base folder
    - SMPL-to-FBX\SMPL_m_unityDoubleBlends_lbs_10_scale5_207_v1.0.0.fbx 
  - copy .pkl file to target folder
    - Source
      - joints2smpl\demo\demo_results\XXX\XXX.pkl
    - Target
      - SMPL-to-FBX\XXX\XXX.pkl
  - execute command
     ```
     python Convert.py --input_pkl_base ./XXX --fbx_source_path SMPL_m_unityDoubleBlends_lbs_10_scale5_207_v1.0.0.fbx --output_base ./
     ```
- Reference
  - [SMPL to FBX](https://github.com/softcat477/SMPL-to-FBX)

## 4) Import FBX on Unity
- Input: SMPL FBX model for Unity (.fbx)
- Procedure
  - import the generated fbx file
    - SMPL-to-FBX\XXX\XXX.fbx
