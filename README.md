loadstring([[
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MobilePanelGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0.8, 0, 0.6, 0)
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner", mainFrame)
corner.CornerRadius = UDim.new(0, 10)

local headPetBtn = Instance.new("TextButton")
headPetBtn.Name = "HeadPetButton"
headPetBtn.Size = UDim2.new(0.5, 0, 0.12, 0)
headPetBtn.Position = UDim2.new(0, 0, 0, 0)
headPetBtn.Text = "Head/Pet"
headPetBtn.Font = Enum.Font.GothamBold
headPetBtn.TextSize = 18
headPetBtn.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
headPetBtn.TextColor3 = Color3.new(1,1,1)
headPetBtn.Parent = mainFrame

local rushPartBtn = Instance.new("TextButton")
rushPartBtn.Name = "RushPartButton"
rushPartBtn.Size = UDim2.new(0.5, 0, 0.12, 0)
rushPartBtn.Position = UDim2.new(0.5, 0, 0, 0)
rushPartBtn.Text = "Rush Part"
rushPartBtn.Font = Enum.Font.GothamBold
rushPartBtn.TextSize = 18
rushPartBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
rushPartBtn.TextColor3 = Color3.new(1,1,1)
rushPartBtn.Parent = mainFrame

local headPetPage = Instance.new("Frame")
headPetPage.Name = "HeadPetPage"
headPetPage.Size = UDim2.new(1, 0, 0.88, 0)
headPetPage.Position = UDim2.new(0, 0, 0.12, 0)
headPetPage.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
headPetPage.Visible = true
headPetPage.Parent = mainFrame

local rushPartPage = Instance.new("Frame")
rushPartPage.Name = "RushPartPage"
rushPartPage.Size = UDim2.new(1, 0, 0.88, 0)
rushPartPage.Position = UDim2.new(0, 0, 0.12, 0)
rushPartPage.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
rushPartPage.Visible = false
rushPartPage.Parent = mainFrame

local label1 = Instance.new("TextLabel")
label1.Text = "Lista de Pets"
label1.Size = UDim2.new(1, 0, 0.2, 0)
label1.BackgroundTransparency = 1
label1.TextColor3 = Color3.new(1,1,1)
label1.Font = Enum.Font.Gotham
label1.TextSize = 20
label1.Parent = headPetPage

local label2 = Instance.new("TextLabel")
label2.Text = "Funções de Rush"
label2.Size = UDim2.new(1, 0, 0.2, 0)
label2.BackgroundTransparency = 1
label2.TextColor3 = Color3.new(1,1,1)
label2.Font = Enum.Font.Gotham
label2.TextSize = 20
label2.Parent = rushPartPage

local function showPage(page)
	if page == "HeadPet" then
		headPetPage.Visible = true
		rushPartPage.Visible = false
	elseif page == "RushPart" then
		headPetPage.Visible = false
		rushPartPage.Visible = true
	end
end

headPetBtn.MouseButton1Click:Connect(function()
	showPage("HeadPet")
	headPetBtn.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
	rushPartBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
end)

rushPartBtn.MouseButton1Click:Connect(function()
	showPage("RushPart")
	headPetBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
	rushPartBtn.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
end)

showPage("HeadPet")
]])()
