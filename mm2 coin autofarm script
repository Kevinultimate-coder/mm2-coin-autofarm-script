local Players = game:GetService("Players")
local CollectionService = game:GetService("CollectionService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

-- Configuration
local MAX_DISTANCE = 150 
local COIN_TAG = "Coin" -- Most common tag in MM2

-- Create GUI for Mobile (Delta)
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MM2AutoFarmGUI"
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Name = "ToggleButton"
frame.Size = UDim2.new(0, 70, 0, 35)
frame.Position = UDim2.new(1, -80, 0, 10) -- Top Right
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 2
frame.BorderColor3 = Color3.fromRGB(0, 0, 0)

local textLabel = Instance.new("TextLabel")
textLabel.Name = "Text"
textLabel.Size = UDim2.new(1, 0, 1, 0)
textLabel.BackgroundTransparency = 1
textLabel.Text = "START"
textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
textLabel.Font = Enum.Font.SourceSansBold
textLabel.TextSize = 16

frame.Parent = screenGui
textLabel.Parent = frame

-- State Variables
local isFarming = false
local connection = nil

-- Function to find coins
local function getCoinsInRange()
	local coins = {}
	for _, part in pairs(workspace:GetChildren()) do
		if CollectionService:HasTag(part, COIN_TAG) then
			local distance = (part.Position - humanoidRootPart.Position).Magnitude
			if distance < MAX_DISTANCE then
				table.insert(coins, part)
			end
		end
	end
	return coins
end

-- Function to teleport to a coin
local function farmCoin(coinPart)
	if not coinPart or not coinPart:IsA("BasePart") then return end
	
	humanoidRootPart.CFrame = CFrame.new(coinPart.Position)
	task.wait(0.05) -- Small delay for smoothness
end

-- Main Farming Loop
local function startFarming()
	isFarming = true
	textLabel.Text = "STOP"
	frame.BackgroundColor3 = Color3.fromRGB(200, 50, 50) -- Red
	
	connection = RunService.Heartbeat:Connect(function()
		if not isFarming then return end
		
		local coins = getCoinsInRange()
		
		if #coins > 0 then
			for _, coin in ipairs(coins) do
				if not isFarming then break end
				farmCoin(coin)
			end
		else
			task.wait(0.1) -- Wait if no coins nearby
		end
	end)
end

local function stopFarming()
	isFarming = false
	textLabel.Text = "START"
	frame.BackgroundColor3 = Color3.fromRGB(50, 200, 50) -- Green
	
	if connection then
		connection:Disconnect()
		connection = nil
	end
end

-- Toggle Function
local function toggleFarm()
	if isFarming then
		stopFarming()
	else
		startFarming()
	end
end

-- Connect the button to the toggle function
frame.MouseButton1Click:Connect(toggleFarm)

print("MM2 Mobile Auto-Farm Loaded. Tap the button to start.")
