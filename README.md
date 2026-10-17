# BNN-PYNQ Integration Baseline for IMECNet

This repository is the legacy [BNN-PYNQ](https://github.com/Xilinx/BNN-PYNQ) framework snapshot used while developing **IMECNet**, a research accelerator for binary neural networks on FPGAs. It is retained as an integration baseline and for historical reproducibility; the IMEC hardware implementation lives in a separate repository.

> **Research artifact:** this code targets the 2018–2020 Xilinx/PYNQ toolchain and is not a maintained production framework. The repository records the environment used for the thesis rather than a current FINN release.

## Project repositories

| Repository | Purpose |
| --- | --- |
| [`imecnet-hlslib`](https://github.com/wady101/imecnet-hlslib) | Core Vivado HLS implementation of Inverse Memory Efficient Convolution (IMEC), the streaming BNN pipeline, testbenches, and generated network parameters. |
| [`imecnet-training-thesis`](https://github.com/wady101/imecnet-training-thesis) | Standalone experimental training and checkpoint-to-HLS conversion work. |
| [`BNN-upstream`](https://github.com/wady101/BNN-upstream) | This repository: the BNN-PYNQ integration baseline and the surrounding PYNQ packaging, notebooks, bitstreams, and host code. |

The checked-in `bnn/src/library/finn-hlslib` directory is a copy of the upstream FINN HLS interface used by this framework snapshot. It is not the IMEC implementation. See `imecnet-hlslib` for `MEC`, the IMEC convolution layer, and the thesis-specific pipeline.

## Artifact scope

The complete research flow was split across the repositories above:

1. Train or adapt the binary Network-in-Network model in `imecnet-training-thesis`.
2. Convert checkpoints into HLS configuration, weights, and thresholds with `cifar/parser_gen.py`.
3. Simulate and synthesize the IMEC pipeline in `imecnet-hlslib`.
4. Use this BNN-PYNQ snapshot for the legacy PYNQ host, packaging, and board-integration context.

There is no current one-command build joining all three archives. Paths and generated files reflect the original research environment and may need adjustment for a local installation.

## Legacy BNN-PYNQ baseline

The baseline contains CNV and LFC accelerators with several weight/activation precisions:

- 1-bit weights and 1-bit activations for CNV and LFC
- 1-bit weights and 2-bit activations for CNV and LFC
- 2-bit weights and 2-bit activations for CNV
- PYNQ-Z1, PYNQ-Z2, and Ultra96 board assets

The original hardware flow was tested with Vivado 2018.2. To rebuild a baseline design:

```bash
git clone https://github.com/wady101/BNN-upstream.git
cd BNN-upstream/bnn/src/network
export XILINX_BNN_ROOT="$(cd .. && pwd)"
./make-hw.sh cnvW1A1 pynqZ1-Z2 a
```

The generated bitstreams, HLS reports, and Vivado projects are written below `bnn/src/network/output/`. Refer to the [upstream BNN-PYNQ repository](https://github.com/Xilinx/BNN-PYNQ) for its maintained historical documentation.

## Citation

If this artifact or the IMEC architecture supports your work, please cite the thesis:

```bibtex
@mastersthesis{wadhwa2020imecbinnets,
  author  = {Wadhwa, Eashan},
  title   = {{IMECBinNets}: A Framework for Deploying Neural Networks on FPGAs},
  school  = {Trinity College Dublin},
  year    = {2020},
  month   = apr,
  doi     = {10.13140/RG.2.2.13870.73287}
}
```

Thesis: [doi:10.13140/RG.2.2.13870.73287](https://doi.org/10.13140/RG.2.2.13870.73287)

## Upstream work

IMECNet was developed from the BNN-PYNQ and FINN ecosystem. Please also cite FINN when using the inherited framework:

```bibtex
@inproceedings{umuroglu2017finn,
  author    = {Umuroglu, Yaman and Fraser, Nicholas J. and Gambardella, Giulio and Blott, Michaela and Leong, Philip and Jahre, Magnus and Vissers, Kees},
  title     = {{FINN}: A Framework for Fast, Scalable Binarized Neural Network Inference},
  booktitle = {Proceedings of the 2017 ACM/SIGDA International Symposium on Field-Programmable Gate Arrays},
  year      = {2017},
  pages     = {65--74},
  publisher = {ACM}
}
```

## License

This BNN-PYNQ snapshot retains the upstream BSD 3-Clause license in [`LICENSE`](LICENSE). Third-party and companion repositories may have different terms.
