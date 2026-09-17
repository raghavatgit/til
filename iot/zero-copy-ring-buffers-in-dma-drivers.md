# Zero-Copy Circular DMA Buffers in Embedded Telemetry

*Date: 2026-09-17*  
*Category: Embedded Systems / Firmware Architecture*

## Overview

When ingesting continuous high-frequency UART or SPI telemetry streams (such as GNSS NMEA sentences or IMU packets) on microcontrollers (e.g. STM32 or ESP32), handling bytes byte-by-byte via CPU interrupts causes interrupt storm overhead and dropped bytes at high baud rates (e.g. 921,600 baud).

## Dual DMA Interrupt Topology

Direct Memory Access (DMA) in circular mode transfers peripheral bytes directly to a static RAM buffer without CPU intervention. By subscribing to two hardware interrupts:
1. **Half-Transfer Complete (HT):** Fires when the first half of the circular buffer (`0` to `BUF_SIZE / 2`) is full.
2. **Transfer Complete (TC):** Fires when the second half of the circular buffer (`BUF_SIZE / 2` to `BUF_SIZE`) is full.

```c
#define BUF_SIZE 512
uint8_t dma_rx_buffer[BUF_SIZE];

void DMA1_Channel5_IRQHandler(void) {
    if (DMA1->ISR & DMA_ISR_HTIF5) {
        DMA1->IFCR = DMA_IFCR_CHTIF5; // Clear HT flag
        // Process first half in background worker
        process_telemetry_chunk(&dma_rx_buffer[0], BUF_SIZE / 2);
    }
    
    if (DMA1->ISR & DMA_ISR_TCIF5) {
        DMA1->IFCR = DMA_IFCR_CTCIF5; // Clear TC flag
        // Process second half in background worker
        process_telemetry_chunk(&dma_rx_buffer[BUF_SIZE / 2], BUF_SIZE / 2);
    }
}
```

## Architectural Advantage
The CPU processes fixed-size contiguous chunks while the DMA hardware continuously populates the opposite half of the buffer, guaranteeing zero dropped bytes and zero CPU interrupt thrashing.
