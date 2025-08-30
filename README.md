-- 🔹 Painel GUI Roblox Lua
local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Name = "PainelCustom"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- Botão Abrir (⚪)
local abrirBtn = Instance.new("TextButton")
abrirBtn.Size = UDim2.new(0, 40, 0, 40)
abrirBtn.Position = UDim2.new(0, 10, 0, 200)
abrirBtn.Text = "O"
abrirBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 255)
abrirBtn.TextScaled = true
abrirBtn.Parent = gui

-- Painel
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 250, 0, 200)
frame.Position = UDim2.new(0.3, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.Visible = false
frame.Parent = gui

-- Botão Fechar (❌)
local fecharBtn = Instance.new("TextButton")
fecharBtn.Size = UDim2.new(0, 30, 0, 30)
fecharBtn.Position = UDim2.new(1, -35, 0, 5)
fecharBtn.Text = "X"
fecharBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
fecharBtn.TextScaled = true
fecharBtn.Parent = frame

-- Botão Ativar/Desativar Auto-Rebater
loca
