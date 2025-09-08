-- Roblox Lua Script: Teleport Panel + Scroll + Drag + Seat support + Auto Teleport Sequencial

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

-- Posições de teleporte
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

-- Painel principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 260, 0, 60)
frame.Position = UDim2.new(0.5, -130, 0.7, 0)
frame.AnchorPoint = Vector2.new(0.5, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BackgroundTransparency = 0.15
frame.BorderSizePixel = 0
frame.ClipsDescendants = true
frame.Parent = gui

local uiCorner = Instance.new("UICorner", frame)
uiCorner.CornerRadius = UDim.new(0, 12)

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -50, 0, 28)
title.Position = UDim2.new(0, 6, 0, 6)
title.BackgroundTransparency = 1
title.Text = "Teleporte — Escolha"
title.TextScaled = true
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.SourceSansSemibold
title.Parent = frame

local openBtn = Instance.new("TextButton")
openBtn.Size = UDim2.new(0, 36, 0, 36)
openBtn.Position = UDim2.new(1, -42, 0, 12)
openBtn.AnchorPoint = Vector2.new(1, 0)
openBtn.Text = "+"
openBtn.Font = Enum.Font.SourceSansBold
openBtn.TextScaled = true
openBtn.BackgroundTransparency = 0.1
openBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
openBtn.TextColor3 = Color3.new(1,1,1)
openBtn.Parent = frame

local openCorner = Instance.new("UICorner", openBtn)
openCorner.CornerRadius = UDim.new(0, 8)

-- ScrollingFrame para lista
local listFrame = Instance.new("ScrollingFrame")
listFrame.Size = UDim2.new(1, 0, 0, 250)
listFrame.Position = UDim2.new(0, 0, 0, 60)
listFrame.BackgroundTransparency = 1
listFrame.BorderSizePixel = 0
listFrame.ScrollBarThickness = 6
listFrame.Visible = false
listFrame.Parent = frame
listFrame.CanvasSize = UDim2.new(0, 0, 0, #teleports * 46 + 50)

local uiListLayout = Instance.new("UIListLayout", listFrame)
uiListLayout.Padding = UDim.new(0, 6)
uiListLayout.SortOrder = Enum.SortOrder.LayoutOrder

-- Teleporte com cadeira
local function teleportTo(pos)
    local char = player.Character or player.CharacterAdded:Wait()
    local hrp = char:WaitForChild("HumanoidRootPart")
    local humanoid = char:FindFirstChildOfClass("Humanoid")

    if humanoid.SeatPart then
        local seat = humanoid.SeatPart
        local target = pos + Vector3.new(0, 3, 0)
        TweenService:Create(seat, tweenInfo, {CFrame = CFrame.new(target)}):Play()
    else
        local target = pos + Vector3.new(0, 3, 0)
        TweenService:Create(hrp, tweenInfo, {CFrame = CFrame.new(target)}):Play()
    end
end

-- Variável de controle do Auto Teleport
local autoTeleporting = false

-- Criar botões de teleport manual
for i, info in ipairs(teleports) do
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(1, -12, 0, 40)
    b.BackgroundTransparency = 0.08
    b.BackgroundColor3 = Color3.fromRGB(60,60,60)
    b.BorderSizePixel = 0
    b.Text = info.name
    b.Font = Enum.Font.SourceSans
    b.TextSize = 18
    b.TextColor3 = Color3.new(1, 1, 1)
    b.Parent = listFrame

    local corner = Instance.new("UICorner", b)
    corner.CornerRadius = UDim.new(0, 8)

    b.MouseButton1Click:Connect(function()
        teleportTo(info.pos)
    end)
end

-- Botão Auto Teleport
local autoBtn = Instance.new("TextButton")
autoBtn.Size = UDim2.new(1, -12, 0, 40)
autoBtn.Position = UDim2.new(0, 6, 0, (#teleports * 46) + 10)
autoBtn.BackgroundTransparency = 0.08
autoBtn.BackgroundColor3 = Color3.fromRGB(80, 50, 50)
autoBtn.BorderSizePixel = 0
autoBtn.Text = "Auto Teleport: OFF"
autoBtn.Font = Enum.Font.SourceSansBold
autoBtn.TextSize = 18
autoBtn.TextColor3 = Color3.new(1, 1, 1)
autoBtn.Parent = listFrame

local corner = Instance.new("UICorner", autoBtn)
corner.CornerRadius = UDim.new(0, 8)

-- Função Auto Teleport Sequencial
autoBtn.MouseButton1Click:Connect(function()
    autoTeleporting = not autoTeleporting
    autoBtn.Text = "Auto Teleport: " .. (autoTeleporting and "ON" or "OFF")
    
    if autoTeleporting then
        task.spawn(function()
            while autoTeleporting do
                for _, info in ipairs(teleports) do
                    if not autoTeleporting then break end
                    teleportTo(info.pos)
                    task.wait(1)
                end
            end
        end)
    end
end)

-- Abrir/fechar painel
local open = false
openBtn.MouseButton1Click:Connect(function()
    open = not open
    if open then
        TweenService:Create(frame, TweenInfo.new(0.25), {Size = UDim2.new(0, 260, 0, 360)}):Play()
        listFrame.Visible = true
        openBtn.Text = "×"
    else
        TweenService:Create(frame, TweenInfo.new(0.25), {Size = UDim2.new(0, 260, 0, 60)}):Play()
        task.delay(0.26, function() listFrame.Visible = false end)
        openBtn.Text = "+"
    end
end)

-- Drag
local dragging, dragStart, startPos
local function updateDrag(input)
    if dragging then
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(
            0, math.clamp(startPos.X.Offset + delta.X, 0, gui.AbsoluteSize.X - frame.Size.X.Offset),
            0, math.clamp(startPos.Y.Offset + delta.Y, 0, gui.AbsoluteSize.Y - frame.Size.Y.Offset)
        )
    end
end

frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = frame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End or input.UserInputState == Enum.UserInputState.Cancel then
                dragging = false
            end
        end)
    end
end)

frame.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        updateDrag(input)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        updateDrag(input)
    end
end)
