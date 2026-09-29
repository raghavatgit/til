# Modern CPU Branch Prediction Architecture

## Components
1. Branch Target Buffer (BTB): Caches target addresses of indirect and direct branches.
2. Direction Predictor: Two-level adaptive predictor using Global History Registers (GHR) and Pattern History Tables (PHT).
3. Return Stack Buffer (RSB): Dedicated LIFO hardware stack predicting `RET` instruction targets.
