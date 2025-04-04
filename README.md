# DRBEST

## Training

#### 1) Transformer

```
cd Transformer
python main.py --name [exp_name] --ckpt_path [save_path] \
               --data_path [training_image_path] \
               --validation_path [validation_image_path] \
               --mask_path [mask_path] \
               --BERT --batch_size 64 --train_epoch 100 \
               --nodes 1 --gpus 8 --node_rank 0 \
               --n_layer [transformer_layer #] --n_embd [embedding_dimension] \
               --n_head [head #] --ImageNet --GELU_2 \
               --image_size [input_resolution]
```

Notes of transformer: 
+ `--AMP`: Reduce the memory cost while training, but sometimes will lead to NAN.
+ `--use_ImageFolder`: Enable this option while training on ImageNet
+ `--random_stroke`: Generate the mask on-the-fly.
+ Our code is also ready for training on multiple machines.

#### 2) Guided Upsampling

```
cd Guided_Upsample
python train.py --model 2 --checkpoints [save_path] \
                --config_file ./config_list/config_template.yml \
                --Generator 4 --use_degradation_2
```

Notes of guided upsampling: 
+ `--use_degradation_2`: Bilinear downsampling. Try to match the transformer training.
+ `--prior_random_degree`: Stochastically deviate the sequence elements by K nearest neighbour.
+ Modify the provided config template according to your own training environments.
+ Training the upsample part won't cost many GPUs.


## Inference

We provide very covenient and neat script for inference.
```
python run.py --input_image [test_image_folder] \
              --input_mask [test_mask_folder] \
              --sample_num 1  --save_place [save_path] \
              --ImageNet --visualize_all
```

Notes of inference: 
+ `--sample_num`: How many completion results do you want?
+ `--visualize_all`: You could save each output result via disabling this option.
+ `--ImageNet` `--FFHQ` `--Places2_Nature`: You must enable one option to select corresponding ckpts.
+ Please use absolute path.

The methodology is derived from an improved version of the [ICT](https://github.com/raywzy/ICT) model adapted to the “anonymization” task, and will be described in more detail once the paper is fully received.
