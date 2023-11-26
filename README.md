# Dacon 3D Number Image Classification
🔗 [Competition Link](https://dacon.io/competitions/official/235951/overview/description)

---

### 🏆 Result:🥇**8/165**🥇

---

## Environment
- Operating System: Ubuntu 18.04
- Package Management: Anaconda
- Deep Learning Framework: Pytorch 1.11

## Description
- Initial attempts with VoxelNet for training struggled to surpass a score of 0.80.
- Inspired by a [post on the code-sharing board](https://dacon.io/competitions/official/235951/codeshare/6476?page=1&dtype=recent), Principal Component Analysis (PCA) was applied to Point Cloud data to convert it into images for training.
- Chose `EfficientNet B0` for its efficiency with simple color schemes and shapes.
- Applied data augmentations: `Rotate`, `RandomGridShuffle`, `Mixup`, and `Backbone Freeze`.
  - `Rotate` was consistently used during training and testing.
  - `RandomGridShuffle` enabled the model to learn high-level features, drawing inspiration from the [JigSaw puzzle concept](https://arxiv.org/pdf/1603.09246.pdf).
  - `Mixup` was used to enhance model performance on ambiguous images.
  - In the final 10 epochs, the model's backbone was frozen, and the classifier was trained without augmentations (except Rotate) to improve performance on non-augmented data.
- Training sequence: `RandomGridShuffle` for 35 epochs, `Mixup` for 25 epochs, followed by `Backbone Freeze` for 10 epochs.
- Used `k-fold`, `Ensemble`, and `Soft Voting` for robustness.
  - Trained on 5 folds.
  - Ensembled the outputs of models from different folds.
  - Compared Soft Voting and Hard Voting; Soft Voting yielded better results.

## Getting Started
- Install Conda virtual environment:
  ```
  conda env create --file environment.yaml
  ```
- Convert Point Cloud Dataset to Image Dataset:
  ```
  python pc2img.py --mode train --data ./data/train.h5 --output ./data/img/train/
  ```

## Exploratory Data Analysis (EDA)
- Refer to `EDA.ipynb`.
- Note: The documentation may be confusing due to incomplete organization.

## Training
- Command to run:
  ```
  python main.py train --CTL_STEP 36 61
  ```
- `CTL_STEP` specifies epochs to apply RandomGridShuffle, Mixup, and Backbone freeze.
- For more details on arguments, see `main.py`.

## Testing
- Command to run:
  ```
  python main.py test --ENSEMBLE soft --CHECKPOINT ./ckpt/model1.pth ./ckpt/model2.pth
  ```
- `ENSEMBLE` option: Use 'soft' for soft voting, 'hard' for hard voting. If not using ensemble, omit this option.
- For additional details on arguments, consult `main.py`.
