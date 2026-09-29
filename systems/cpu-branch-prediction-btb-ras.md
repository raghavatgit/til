# Modern CPU Branch Prediction: BTB, TAGE, and RAS

Modern speculative out-of-order execution pipelines predict branch directions to sustain instruction decode rates.

## Structures
- **BTB (Branch Target Buffer)**: Caches target jump addresses for unconditional and taken conditional branches.
- **TAGE Predictor**: State-of-the-art branch predictor utilizing geometric history lengths and tagged hash tables.
- **RAS (Return Address Stack)**: Dedicated hardware circular stack predicting function return addresses.
