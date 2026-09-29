# TCP Window Scaling (RFC 7323)

## The 16-Bit Window Limit
The original TCP header reserves 16 bits for the receive window size, capping buffer advertisements at 65,535 bytes (64 KB).

## Bandwidth-Delay Product (BDP)
$$BDP = \text{Bandwidth} \times \text{RTT}$$
On a 10 Gbps transcontinental fiber link with 80ms RTT:
$$BDP = \frac{10 \times 10^9}{8} \times 0.080 = 100\text{ MB}$$
A 64 KB window utilizes less than 0.064% of available bandwidth.

## Window Scale Option
Negotiated during SYN exchange: specifies a scale factor $S$ (0 to 14).
The actual receive window is computed as:
$$\text{Effective Window} = \text{Window Value} \times 2^S$$
Enables window sizes up to 1 GB ($65,535 \times 2^{14}$).
