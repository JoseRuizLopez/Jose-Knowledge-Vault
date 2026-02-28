links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# LoRA — Low-Rank Adaptation

Técnica de fine-tuning eficiente que congela los pesos originales y añade matrices de rango bajo entrenables: ΔW = BA con r ≪ min(d, k).

## Ventajas

- <1% de parámetros entrenables respecto al modelo base.
- Fine-tuning en GPUs con poca VRAM.
- Adaptadores intercambiables.

## Variantes

- **QLoRA**: LoRA + cuantización 4 bits.
- **DoRA**: descompone pesos en magnitud y dirección antes de aplicar LoRA.


---
tags:
	#LLM #FineTuning #PEFT