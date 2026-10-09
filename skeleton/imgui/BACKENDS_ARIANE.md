# Dear ImGui integration

Core and Win32/DX9 backends: v1.92.2b-docking, commit
`1f7f1f54af38b0350d8c0008b096a9af6de299c7` from ocornut/imgui (MIT;
see LICENSE_imgui.txt). Existing librw imconfig.h and ImGuizmo are retained.

Local DX9 backend changes: optional application texture-ID resolver (font
textures bypass it); tolerate failed secondary swap-chain creation during device
loss and retry on CreateDeviceObjects. Applications must invalidate backend
resources using rw::d3d::releaseDeviceResources before device resets, recreate
objects after recovery, and render platform windows inside BeginScene/EndScene.
The new backends are opt-in; the existing RW backend remains the default.
