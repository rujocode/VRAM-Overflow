# VRAM-Overflow
VRAM Overflow es una herramienta que extiende virtualmente la capacidad de memoria de tu GPU, utilizando la RAM del sistema o la iGPU como reserva inteligente cuando se agota la VRAM dedicada. Ideal para jugadores con hardware limitado o juegos pesados en texturas

## Objetivo General
Crear una herramienta que expanda virtualmente la VRAM disponible en una GPU dedicada, redirigiendo ciertos recursos (texturas, UI) a buffers alternativos en RAM o iGPU cuando se detecta overflow de memoria.

---

## ⚖Arquitectura Modular

| Módulo                | Tecnología / Lenguaje        | Rol Principal |
|-------------------------|-------------------------------|----------------|
| GUI (Frontend)          | WPF (.NET 6/7, C#)            | Configuración e inyección |
| Injector DLL (Backend)  | C++ + MinHook + DirectX 11    | Hook de recursos y lógica de redirección |
| Configuración JSON     | Archivo config.json en AppData| Comunicación simple entre GUI y DLL |

---

## ✨ Funcionalidades Completas Hasta Ahora

### Interfaz Gráfica (GUI - WPF)
- Selector de ejecutable
- Checkboxes: habilitar Overflow, RAM, iGPU
- Sliders: uso máximo de RAM, umbral de VRAM
- Simulación visual de uso de VRAM / RAM / iGPU
- Botón de inyección: lanza el juego y ejecuta el injector

### Inyector de DLL (C++)
- Hook a `CreateTexture2D` (DirectX 11)
- Lógica para redirigir recursos grandes si VRAM supera umbral
- Lectura de configuración JSON guardada por el frontend
- Uso de **NvAPI** para leer VRAM usada en GPUs NVIDIA

---

## Configuración (config.json)
Guardado automáticamente por el frontend en:
```plaintext
%APPDATA%\VRAMOverflow\config.json
```
Ejemplo:
```json
{
  "EnableOverflow": true,
  "UseRAM": true,
  "UseIGPU": false,
  "OverflowThreshold": 90,
  "MaxRAMUsageGB": 2,
  "RedirectTextures": true,
  "RedirectUI": true,
  "RedirectGeometry": false
}
```

---

## ⚖️ Tecnologías Usadas
- C++ 17 / C# .NET 6 / Visual Studio 2022+
- DirectX 11 (hook a nivel de API)
- MinHook (hooking en C++)
- NvAPI (uso de VRAM en GPUs NVIDIA)
- nlohmann/json (lectura de config.json)

---

## ⚡ Pendientes / Roadmap sugerido
- [ ] Medición de VRAM total (no solo usada)
- [ ] Implementar buffer en RAM/iGPU (para redirigir datos)
- [ ] Placeholder de textura en VRAM (para evitar fallos)
- [ ] Perfilado de rendimiento
- [ ] Compatibilidad con Vulkan y DX12
- [ ] Modo "Safe Monitor Only" (sin hook)

---

## ✉ Comentarios Finales
Este proyecto combina bajo nivel (hooking) con alto nivel (WPF) y está diseñado para ser modular, escalable y seguro. Ya es posible probar todo el flujo funcional desde GUI hasta hook activo.

> Avanzar paso a paso, como hasta ahora, es clave para mantener la estabilidad del sistema. 

---

✅ **Siguiente paso recomendado**: Implementar la creación de texturas dummy en VRAM y buffers reales en RAM/iGPU.
