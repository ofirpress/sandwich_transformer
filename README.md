
## Improving Transformer Models by Reordering their Sublayers

This repository contains the code for running the character-level **Sandwich Transformers** from our ACL 2020 paper on [Improving Transformer Models by Reordering their Sublayers](https://ofir.io/sandwich_transformer.pdf) (video presentation [here](https://www.youtube.com/watch?v=rFuuGEj3AhU), summary [here](https://ofir.io/Improving-Transformer-Models-by-Reordering-their-Sublayers/)). 

Our character-level model (and this repo) is based on the  [Adaptive Attention Span for Transformers](https://arxiv.org/abs/1905.07799) model. In our paper we showed that by simply reordering that model's self-attention and feedforward sublayers, we could improve performance on the enwik8 benchmark (where we achieve 0.968 BPC on the test set). 

The code here simply adds a way to reorder the sublayers of the Adaptive Span model, using the `--architecture` parameter. 



## Requirements
You need CUDA 10 and PyTorch 1.2.0 to run this code. See [this page](https://pytorch.org/get-started/previous-versions/https://pytorch.org/get-started/previous-versions/) for installation instructions. To replicate our experimental conditions eight V100 GPUs are needed. 

## Running experiments in the paper
The scripts for training the character-level models from the paper are located in the `./experiments/` directory. For example, to train the enwik8 model, run:
```bash
bash experiments/enwik8_large.sh
```

We used eight V100 GPUs, but if you'd like to run this model on GPUs with less memory you can increase the `--batch-split`  (it splits batches into smaller pieces without changing the final result).

We  obtained the following results in our experiments:

| Experiment | #params | valid (bpc) | test (bpc) |
| ---------- | ---:|            ----:|       ----:|
| **enwik8 Sandwich Transformer**| 209M |  0.992 | 0.968 |
| **text8 Sandwich Transformer** | 209M |  1.012 | 1.076 |


### The `--architecture` parameter
A standard transformer with 3 layers (so 6 self-attention and feedforward sublayers) would use be trained using  `--architecture sfsfsf`. That 6 sublayer model with a sandwiching coefficient of 1 would be  `--architecture s.sfsf.f` and with a sandwiching coefficient of 2 would be  `--architecture s.s.sf.f.f`. Make sure to also set the `--nlayers` parameter to be the length of the `architecture` string divided by 2. 


## License
The code is licensed under CC-BY-NC license. See the LICENSE file for more details.

## Acknowledgements + More Information
This code is based on the code of the [Adaptive Span]([https://github.com/facebookresearch/adaptive-span](https://github.com/facebookresearch/adaptive-span)) model. We recommend reading the [Adaptive Span README](https://github.com/facebookresearch/adaptive-span/blob/master/README.md) for further information on this codebase. 
