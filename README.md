-- Roblox Lua Script: Teleport Panel + Auto Teleport 3 "cliques" por posição

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local gui = Instance.new("ScreenGui")
gui.Name = "TeleportGUI"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Parent = playerGui

local tweenInfo = TweenInfo.new(0.35, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

-- Lista de teleportes
local teleports = {
    {name = "Condenada 1", pos = Vector3.new(4253.15, 29.67, -6964.59)},
    {name = "Condenada 2", pos = Vector3.new(4299.07, 44.31, -6897.23)},
    {name = "Condenada 3", pos = Vector3.new(4345.58, 74.50, -7019.68)},
    {name = "Condenada 4", pos = Vector3.new(4437.02, 95.86, -6966.37)},
    {name = "Condenada 5", pos = Vector3.new(4499.72, 104.85, -7015.65)},
    {name = "Condenada 6", pos = Vector3.new(4450.35, 106.52, -7039.21)},
    {name = "Condenada 7", pos = Vector3.new(4398.37, 85.95, -6977.11)},
    {name = "Condenada 8", pos = Vector3.new(4346.50, 71.56, -7018.40)},
    {name = "Condenada 9", pos = Vector3.new(4305.84, 43.87, -6898.48)},
    {name = "Condenada 10", pos = Vector3.new(4260.06, 27.13, -6960.53)},
    {name = "Condenada 11", pos = Vector3.new(4192.87, 21.01, -6990.53)}
}

-- Painel
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 260, 0, 60)
frame.Position = UDim2.new(0.5, -130, 0.7, 0)
frame.AnchorPoint = Vector2.new(0.5, 0)
frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
frame.BackgroundTransparency = 0.15
frame.BorderSizePixel = 0
frame.ClipsDescendants = true
frame.Parent = gui
Instance.new("UICorner", frame).CornerRadius = UDim.new(0,12)

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -50, 0, 28)
title.Position = UDim2.new(0,6,0,6)
title.BackgroundTransparency = 1
title.Text = "Teleporte — Escolha"
title.TextScaled
