-- GUI com botão para mostrar/ocultar a janela principal
-- Feito para Ryan

local player = game.Players.LocalPlayer

-- Criação da tela
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "FrutaToggleGUI"
gui.ResetOnSpawn = false

-- FRAME PRINCIPAL
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 260, 0, 300)
mainFrame.Position = UDim2.new(0, 20, 0.3, 0)
mainFrame.BackgroundColor3 = Color3.new(0, 0, 0) -- Preto
mainFrame.BackgroundTransparency = 0.3 -- Levemente transparente
mainFrame.BorderSizePixel = 0
mainFrame.Visible = true

-- TÍTULO
local titulo = Instance.new("TextLabel", mainFrame)
titulo.Size = UDim2.new(1, 0, 0, 30)
titulo.Text = "Frutas"
titulo.TextColor3 = Color3.new(1, 1, 1)
titulo.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
titulo.Font = Enum.Font.GothamBold
titulo.TextScaled = true

-- FRAME de botões de fruta
local dropFrame = Instance.new("Frame", mainFrame)
dropFrame.Position = UDim2.new(0, 10, 0, 40)
dropFrame.Size = UDim2.new(0, 240, 0, 140)
dropFrame.BackgroundTransparency = 1

-- Lista de frutas
local frutas = {
	"Cenoura",
	"Mirtilo",
	"Morango",
	"Laranja",
	"Caneta Azul"
}

-- Variável fruta selecionada
local frutaSelecionada = nil

-- Criar botões de frutas
for i, fruta in ipairs(frutas) do
	local botao = Instance.new("TextButton", dropFrame)
	botao.Size = UDim2.new(1, 0, 0, 25)
	botao.Position = UDim2.new(0, 0, 0, (i - 1) * 28)
	botao.Text = fruta
	botao.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
	botao.TextColor3 = Color3.new(1,1,1)
	botao.Font = Enum.Font.GothamBold
	botao.TextScaled = true
	botao.MouseButton1Click:Connect(function()
		frutaSelecionada = fruta
	end)
end

-- Botão: Comprar fruta
local botaoComprar = Instance.new("TextButton", mainFrame)
botaoComprar.Size = UDim2.new(0, 240, 0, 40)
botaoComprar.Position = UDim2.new(0, 10, 0, 190)
botaoComprar.Text = "✅ Comprar Fruta Selecionada"
botaoComprar.BackgroundColor3 = Color3.fromRGB(80, 150, 80)
botaoComprar.TextColor3 = Color3.new(1,1,1)
botaoComprar.Font = Enum.Font.GothamBold
botaoComprar.TextScaled = true

-- Remote para comprar
local rs = game:GetService("ReplicatedStorage")
local eventos = rs:WaitForChild("Eventos")
local remoteComprar = eventos:WaitForChild("ComprarSemente")

botaoComprar.MouseButton1Click:Connect(function()
	if frutaSelecionada then
		remoteComprar:FireServer(frutaSelecionada)
	else
		warn("Nenhuma fruta selecionada!")
	end
end)

-- Botão: Vender
local botaoVender = Instance.new("TextButton", mainFrame)
botaoVender.Size = UDim2.new(0, 240, 0, 40)
botaoVender.Position = UDim2.new(0, 10, 0, 240)
botaoVender.Text = "💰 Vender Tudo"
botaoVender.BackgroundColor3 = Color3.fromRGB(180, 80, 80)
botaoVender.TextColor3 = Color3.new(1,1,1)
botaoVender.Font = Enum.Font.GothamBold
botaoVender.TextScaled = true

local remoteVender = eventos:WaitForChild("Vender"):WaitForChild("Vender inventário")

botaoVender.MouseButton1Click:Connect(function()
	remoteVender:FireServer()
end)

-- Botão "GUI" para esconder/mostrar a GUI principal
local toggleButton = Instance.new("TextButton", gui)
toggleButton.Size = UDim2.new(0, 60, 0, 30)
toggleButton.Position = UDim2.new(0, 290, 0.3, 0) -- ao lado do frame principal
toggleButton.Text = "GUI"
toggleButton.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
toggleButton.BackgroundTransparency = 0.2
toggleButton.TextColor3 = Color3.new(1,1,1)
toggleButton.Font = Enum.Font.GothamBold
toggleButton.TextScaled = true

-- Alternar visibilidade do frame principal
toggleButton.MouseButton1Click:Connect(function()
	mainFrame.Visible = not mainFrame.Visible
end)
