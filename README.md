-- LocalScript em StarterPlayerScripts
-- Cria contornos finos (Highlight) nos jogadores do jogo, respeitando paredes (sem wallhack)

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Configuração do contorno
local OUTLINE_COLOR = Color3.fromRGB(255, 255, 255) -- branco
local FILL_TRANSPARENCY = 1                          -- só contorno (sem preenchimento)
local DEPTH_MODE = Enum.HighlightDepthMode.Occluded  -- respeita paredes (sem ver através)

-- Cria (ou reutiliza) um Highlight para o modelo passado
local fu
