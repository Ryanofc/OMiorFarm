local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Eventos = ReplicatedStorage:WaitForChild("Eventos")
local ComprarSemente = Eventos:WaitForChild("ComprarSemente")
local VenderInventario = Eventos:WaitForChild("Vender"):WaitForChild("Vender inventário")

local frutasDisponiveis = {
    "Cenoura", "Mirtilo", "Laranjeira", "Caneta Azul",
    "Marshmallow", "Pistache", "Mango", "Uva", "Cuscuz"
}

-- GUI
local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
screenGui.Name = "FrutaGui"

local mainFrame = Instance.new("Frame", screenGui)
mainFrame.Size = UDim2.new(0, 300, 0, 250)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -125)
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.BorderSizePixel = 0

-- Vender com switch visual
local venderAtivo = false

local switchContainer = Instance.new("Frame", mainFrame)
switchContainer.Size = UDim2.new(0.5, -15, 0, 40)
switchContainer.Position = UDim2.new(0, 10, 0, 10)
switchContainer.BackgroundTransparency = 1

local switchBase = Instance.new("Frame", switchContainer)
switchBase.Size = UDim2.new(0, 60, 0, 25)
switchBase.Position = UDim2.new(0, 0, 0.5, -12)
switchBase.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
switchBase.BorderSizePixel = 0
switchBase.Name = "SwitchBase"
switchBase.ClipsDescendants = true

local switchBall = Instance.new("Frame", switchBase)
switchBall.Size = UDim2.new(0, 23, 0, 23)
switchBall.Position = UDim2.new(0, 1, 0, 1)
switchBall.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
switchBall.BorderSizePixel = 0
switchBall.ZIndex = 3
switchBall.Name = "SwitchBall"
switchBall.AnchorPoint = Vector2.new(0, 0)
switchBall.SizeConstraint = Enum.SizeConstraint.RelativeYY
switchBall.ClipsDescendants = false

local venderLabel = Instance.new("TextLabel", switchContainer)
venderLabel.Size = UDim2.new(1, -70, 1, 0)
venderLabel.Position = UDim2.new(0, 70, 0, 0)
venderLabel.BackgroundTransparency = 1
venderLabel.Text = "Vender: OFF"
venderLabel.TextColor3 = Color3.new(1,1,1)
venderLabel.TextScaled = true
venderLabel.Font = Enum.Font.Gotham

local function atualizarSwitch()
	if venderAtivo then
		switchBase.BackgroundColor3 = Color3.fromRGB(80, 255, 80)
		switchBall:TweenPosition(UDim2.new(1, -24, 0, 1), "Out", "Quad", 0.2, true)
		venderLabel.Text = "Vender: ON"
	else
		switchBase.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
		switchBall:TweenPosition(UDim2.new(0, 1, 0, 1), "Out", "Quad", 0.2, true)
		venderLabel.Text = "Vender: OFF"
	end
end

switchBase.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		venderAtivo = not venderAtivo
		atualizarSwitch()
	end
end)

task.spawn(function()
	while true do
		if venderAtivo then
			VenderInventario:FireServer()
		end
		task.wait(2)
	end
end)

atualizarSwitch()

-- Botão Select Fruit
local selectBtn = Instance.new("TextButton", mainFrame)
selectBtn.Size = UDim2.new(0.5, -15, 0, 40)
selectBtn.Position = UDim2.new(0.5, 5, 0, 10)
selectBtn.Text = "Select Fruit"
selectBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 255)
selectBtn.TextColor3 = Color3.new(1,1,1)

-- Lista de frutas
local listFrame = Instance.new("Frame", mainFrame)
listFrame.Size = UDim2.new(1, -20, 0, 120)
listFrame.Position = UDim2.new(0, 10, 0, 60)
listFrame.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
listFrame.Visible = false
listFrame.ClipsDescendants = true

local selectedFruit = nil

for i, fruta in ipairs(frutasDisponiveis) do
	local btn = Instance.new("TextButton", listFrame)
	btn.Size = UDim2.new(1, 0, 0, 20)
	btn.Position = UDim2.new(0, 0, 0, (i - 1) * 20)
	btn.Text = fruta
	btn.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
	btn.TextColor3 = Color3.new(1,1,1)
	
	btn.MouseButton1Click:Connect(function()
		selectedFruit = fruta
		selectBtn.Text = "Selecionado: " .. fruta
		listFrame.Visible = false
	end)
end

selectBtn.MouseButton1Click:Connect(function()
	listFrame.Visible = not listFrame.Visible
end)

-- Botão Buy
local buyBtn = Instance.new("TextButton", mainFrame)
buyBtn.Size = UDim2.new(1, -20, 0, 40)
buyBtn.Position = UDim2.new(0, 10, 1, -50)
buyBtn.Text = "Buy"
buyBtn.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
buyBtn.TextColor3 = Color3.new(0,0,0)

buyBtn.MouseButton1Click:Connect(function()
	if selectedFruit then
		local args = {selectedFruit}
		ComprarSemente:FireServer(unpack(args))
	else
		buyBtn.Text = "Selecione uma fruta"
		wait(1.5)
		buyBtn.Text = "Buy"
	end
end)
