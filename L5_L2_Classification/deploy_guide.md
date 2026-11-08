# Deploy Guide — L_OPENVINO
**Tier:** TIER_9_ROBOTICS_IOT | **Stack:** Python 3.11, openvino 2024.x, K_OPENVINO, PAX 27B, AIOSS_FORMAT
**Air-gap capable after initial setup.**

## Prerequisites
Python 3.11+, openvino 2024.x, Intel CPU/iGPU devices. OTA update infrastructure.

## Environment
Intel-based edge devices. OTA server. 16GB RAM management server. T4 GPU for PAX approvals.

## AIOSS Integration
```bash
aioss init --module L_OPENVINO --output ./l_openvino.aioss
aioss append --chain ./l_openvino.aioss --payload ./output.bin --module L_OPENVINO
aioss verify --chain ./l_openvino.aioss
```

## Air-Gap Setup
```bash
pip download -r requirements.txt -d ./wheels/
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="L_OPENVINO",
    aioss_chain="./L_OPENVINO.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./L_OPENVINO.aioss --verbose
python -m L_OPENVINO.tests.smoke
```
