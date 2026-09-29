# Linux Network Stack: NAPI Polling and SoftIRQs

## Interrupt Storms
At 10 Gbps and above, generating a hardware interrupt for every incoming packet overwhelms the CPU, spending all compute cycles servicing IRQ handlers (Receive Livelock).

## NAPI (New API) Framework
1. When the first packet arrives, the network interface card raises a hardware interrupt.
2. The kernel interrupt handler deschedules future NIC hardware interrupts and schedules a software interrupt (`NET_RX_SOFTIRQ`).
3. Driver executes in polling mode: Drains the NIC DMA ring buffer up to a budget (default 64 packets per poll cycle) and passes frames up the networking stack.
4. Once the ring buffer is drained, NIC hardware interrupts are re-enabled.
