local scriptString = [[
    local PixelData = {
        {{255,0,0},{0,255,0},{0,0,255}},
        {{255,255,0},{255,0,255},{0,255,255}},
    }
    
    local blockSize = 1
    local startPosition = Vector3.new(0,5,0)
    local folder = Instance.new("Folder", workspace)
    folder.Name = "PixelArt"

    -- Criar GUI
    local player = game.Players.LocalPlayer
    local playerGui = player:WaitForChild("PlayerGui")
    local screenGui = Instance.new("ScreenGui", playerGui)
    screenGui.Name = "PaintGui"

    local button = Instance.new("TextButton", screenGui)
    button.Size = UDim2.new(0.3,0,0.1,0)
    button.Position = UDim2.new(0.35,0,0.85,0)
    button.Text = "Pintar Arte"
    button.BackgroundColor3 = Color3.fromRGB(51,153,230)
    button.Font = Enum.Font.GothamBold
    button.TextSize = 24

    button.MouseButton1Click:Connect(function()
        folder:ClearAllChildren()
        for y=1,#PixelData do
            for x=1,#PixelData[y] do
                local c = PixelData[y][x]
                local color = Color3.fromRGB(c[1], c[2], c[3])
                local part = Instance.new("Part")
                part.Size = Vector3.new(blockSize, blockSize, blockSize)
                part.Anchored = true
                part.Position = startPosition + Vector3.new((x-1)*blockSize, 0, (y-1)*blockSize)
                part.Color = color
                part.Parent = folder
