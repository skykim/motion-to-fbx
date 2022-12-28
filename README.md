# motion-to-fbx
- How to convert motion text to SMPL FBX file for Unity
- 텍스트로 부터 추출된 에니메이션 모션을 SMPL 모델의 FBX 파일로 Retargeting하여 Unity에서 Import하는 방법

# Demo

- A person stretches both hands up to the sky, then lowers them
  - num_smplify_iters: 20

   ![ezgif-2-618ea338da](https://user-images.githubusercontent.com/18140805/209832183-1013fe0a-598f-482f-bbc7-eb9e8d8dabef.gif)

- A person swimming forward.
  - num_smplify_iters: 50

  ![ezgif-2-e92593d1e9](https://user-images.githubusercontent.com/18140805/209837946-cd815f26-588c-4549-996a-204d5af09f7c.gif)


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
     python fit_seq.py --files XXX.npy --num_smplify_iters 20
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
    
# Problems

- :warning: Now, animations are locked in place. It will be fixed in the future.

# Appendix

 ## A map of 22 joints (comparing with SMPL)
- 'MidHip': 0
- 'LHip': 1, 'LKnee': 4, 'LAnkle': 7, 'LFoot': 10
- 'RHip': 2, 'RKnee': 5, 'RAnkle': 8, 'RFoot': 11
- 'LShoulder': 16, 'LElbow': 18, 'LWrist': 20
- 'RShoulder': 17, 'RElbow': 19, 'RWrist': 21
- 'spine1': 3, 'spine2': 6, 'spine3': 9,  'Neck': 12, 'Head': 15
- 'LCollar':13, 'Rcollar' :14

 ![image](https://user-images.githubusercontent.com/18140805/209820687-4334b9ab-84d2-4be4-bce2-a73a5f4570d7.png)
