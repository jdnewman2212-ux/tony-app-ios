NOTICE — «Tony», un fork de PocketPal AI
=========================================

«Tony» es una personalización privada, sin fines de distribución comercial,
de **PocketPal AI**, creada por José Daniel Márquez Newman para su uso
personal en su propio iPhone.

Proyecto original:
  PocketPal AI — https://github.com/a-ghorbani/pocketpal-ai
  Copyright (c) 2024 Asghar Ghorbani y colaboradores
  Licencia MIT (ver el archivo LICENSE de esta carpeta, conservado intacto
  del proyecto original, tal como exige la licencia).

Base utilizada: rama `main` de PocketPal AI, commit
`ed0b9654b3be0123a36712c0923dc3d6d41cac21` (2026-11-19), versión 1.24.0.

Cambios de este fork (detalle completo en STATUS.md):
  - Nombre visible «Tony», bundle id iOS `com.josedaniel.tony`.
  - Tema oscuro con los colores de Tony (rojo candy apple #C8102E, dorado
    champagne #C9A227, cian #22D3EE).
  - La cara de Tony (avatar propio del usuario) como icono de la app y
    como imagen del asistente en la app.
  - Pal por defecto «Tony» con persona en español para uso offline de viaje.
  - Modelo recomendado por defecto: Qwen3-4B Q4_K_M (repo oficial
    Qwen/Qwen3-4B-GGUF en Hugging Face, licencia Apache-2.0 de Qwen).

El motor de inferencia es llama.cpp (proyecto ggml-org, licencia MIT) a
través de las dependencias que ya trae PocketPal AI.

Este fork no está afiliado ni respaldado por PocketPal AI, Qwen/Alibaba,
Apple ni Meta. «Tony» es un nombre de uso personal.
