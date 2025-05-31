-- GUI de Frutas com botão toggle para Delta
local player = game:GetService("Players").LocalPlayer
repeat wait() until player:FindFirstChild("PlayerGui")

-- Criar GUI principal
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "FrutaGUI"
gui.ResetOnSpawn = false

-- Frame principal
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 260, 0, 300)
mainFrame.Position = UDim2.new(0, 20, 0.3, 0)
mainFrame.BackgroundColor3 = Color3.new(0, 0, 0)
mainFrame.BackgroundTransparency = 0.3
mainFrame.BorderSizePixel = 0
mainFrame.Visible = true

-- Título
local titulo = Instance.new("TextLabel", mainFrame)
titulo.Size = UDim2.new(1, 0, 0, 30)
titulo.Text = "Frutas"
titulo.TextColor3 = Color3.new(1, 1, 1)
titulo.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
titulo.Font = Enum.Font.GothamBold
titulo.TextScaled = true

-- Lista de frutas
local frutas = {"Cajá", "Mirtilo", "Marshmallow", "Laranjeira", "Caneta Azul"}
local frutaSelecionada = nil

-- Frame com botões de frutas
local dropFrame = Instance.new("Frame", mainFrame)
dropFrame.Position = UDim2.new(0, 10, 0, 40)
dropFrame.Size = UDim2.new(0, 240, 0, 140)
dropFrame.BackgroundTransparency = 1

for i, fruta in ipairs(frutas) do
	local botao = Instance.new("TextButton", dropFrame)
	botao.Size = UDim2.new(1, 0, 0, 25)
	botao.Position = UDim2.new(0, 0, 0, (i - 1) * 28)
	botao.Text = fruta
	botao.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
	botao.TextColor3 = Color3.new(1, 1, 1)
	botao.Font = Enum.Font.GothamBold
	botao.TextScaled = true
	botao.MouseButton1Click:Connect(function()
		frutaSelecionada = fruta
	end)
end

-- Botão de Comprar
local botaoComprar = Instance.new("TextButton", mainFrame)
botaoComprar.Size = UDim2.new(0, 240, 0, 40)
botaoComprar.Position = UDim2.new(0, 10, 0, 190)
botaoComprar.Text = "✅ Comprar Fruta Selecionada"
botaoComprar.BackgroundColor3 = Color3.fromRGB(80, 150, 80)
botaoComprar.TextColor3 = Color3.new(1, 1, 1)
botaoComprar.Font = Enum.Font.GothamBold
botaoComprar.TextScaled = true

botaoComprar.MouseButton1Click:Connect(function()
	if frutaSelecionada then
		local rs = game:GetService("ReplicatedStorage")
		local eventos = rs:WaitForChild("Eventos")
		local remoteComprar = eventos:WaitForChild("ComprarSemente")
		remoteComprar:FireServer(frutaSelecionada)
	else
		warn("Nenhuma fruta selecionada.")
	end
end)

-- Botão de Vender
local botaoVender = Instance.new("TextButton", mainFrame)
botaoVender.Size = UDim2.new(0, 240, 0, 40)
botaoVender.Position = UDim2.new(0, 10, 0, 240)
botaoVender.Text = "💰 Vender Tudo"
botaoVender.BackgroundColor3 = Color3.fromRGB(180, 80, 80)
botaoVender.TextColor3 = Color3.new(1, 1, 1)
botaoVender.Font = Enum.Font.GothamBold
botaoVender.TextScaled = true

botaoVender.MouseButton1Click:Connect(function()
	local rs = game:GetService("ReplicatedStorage")
	local eventos = rs:WaitForChild("Eventos")
	local remoteVender = eventos:WaitForChild("Vender"):WaitForChild("Vender inventário")
	remoteVender:FireServer()
end)

-- Botão de Toggle da GUI
local toggleButton = Instance.new("TextButton", gui)
toggleButton.Size = UDim2.new(0, 60, 0, 30)
toggleButton.Position = UDim2.new(0, 290, 0.3, 0)
toggleButton.Text = "GUI"
toggleButton.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
toggleButton.BackgroundTransparency = 0.2
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.Font = Enum.Font.GothamBold
toggleButton.TextScaled = true

toggleButton.MouseButton1Click:Connect(function()
	mainFrame.Visible = not mainFrame.Visible
end)
