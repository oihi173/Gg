-- Cria uma ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "TeleportGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = PlayerGui panel.Parent = PlayerGui local player = game.Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Cria uma ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "TeleportGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = PlayerGui
-- Painel principal
local panel = Instance.new("Frame")
panel.Name = "TeleportPanel"
panel.Size = UDim2.new(0, 220, 0, 220)
panel.Position = UDim2.new(0, 20, 0, 100)
panel.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
panel.BackgroundTransparency = 0.1
panel.BorderSizePixel = 0
panel.Active = true
panel.Draggable = true
panel.Parent = screenGui
