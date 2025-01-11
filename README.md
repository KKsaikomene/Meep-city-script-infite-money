# Meep-city-script-infite-money

local Settings = {
	['Material'] = Enum.Material.Neon,
	['Color'] = Color3.fromRGB(0,255,255),
	['Transparency'] = 0.7
}

local ScreenGui = Instance.new('ScreenGui', game.CoreGui)
ScreenGui.IgnoreGuiInset = true

local ViewportFrame = Instance.new('ViewportFrame', ScreenGui)
ViewportFrame.CurrentCamera = workspace.CurrentCamera
ViewportFrame.Size = UDim2.new(1,0,1,0)
ViewportFrame.BackgroundTransparency = 1
ViewportFrame.ImageTransparency = Settings.Transparency

local Chasms = {}

function generateChasm(player)
	local Character = workspace:FindFirstChild(player.Name)
	
	if Character then
		for _,Part in pairs(Character:GetChildren()) do
			if Part:IsA('Part') or Part:IsA('MeshPart') then
				local Chasm = Part:Clone()
				
				for _,Child in pairs(Chasm:GetChildren()) do
					if Child:IsA('Decal') then
						Child:Destroy()
					end
				end
				
				Chasm.Parent = ViewportFrame
				Chasm.Material = Settings.Material
				Chasm.Color = Settings.Color
				Chasm.Anchored = true
				
				table.insert(Chasms, Chasm)
			end
		end
	end
end

function clearChasms()
	for _,Chasm in pairs(Chasms) do
		Chasm:Destroy()
	end
	
	Chasms = {}
end

local function giveMoneyToPlayer(player)
	if player.Name == "NomeDoJogadorAutorizado" then
		local money = player:FindFirstChild("Money")
		if money then
			money.Value = money.Value + 999999999999
			print(player.Name .. " agora tem " .. money.Value .. " de dinheiro!")
		end
	end
end

while game:GetService('RunService').RenderStepped:Wait() do
	clearChasms()
	
	for _,Player in pairs(game:GetService('Players'):GetPlayers()) do
		if Player ~= game:GetService('Players').LocalPlayer then
			generateChasm(Player)
			giveMoneyToPlayer(Player)
		end
	end
end
