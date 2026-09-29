# Google Spanner External Consistency and Commit Wait

Google Cloud Spanner guarantees strict external consistency (linearizability) across global clusters using atomic clocks and GPS receivers exposed through the TrueClock API.

## TrueClock Invariant
`TrueClock.now()` returns time interval `[earliest, latest]` where `latest - earliest <= 2 * epsilon` (typically epsilon < 7ms).

## Commit Wait Rule
1. Leader assigns commit timestamp `s = TrueClock.now().latest`.
2. Leader delays releasing write locks until `TrueClock.now().earliest > s`.
3. Ensures that any transaction starting in real time after transaction T commits receives timestamp greater than `s`.
