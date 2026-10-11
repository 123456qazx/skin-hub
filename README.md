local clock = os.clock
local clock2 = os.clock
local floor = math.floor
local abs = math.abs
local min = math.min
local vector = Vector3.new
local vector2 = Vector2.new
local color = Color3.fromRGB
local cframe = CFrame.new
local format = string.format

local function fn()
	local v = cloneref
	if not v then
		return function(arg)
			return arg
		end
	end
	local obj = setmetatable({}, { __mode = "v" })

	return function(arg)
		if not arg then
			return nil
		end
		local debugId = arg:GetDebugId()
		local v2 = obj[debugId]
		if v2 then
			return v2
		end
		local v3 = v(arg)
		obj[debugId] = v3
		return v3
	end
end

local v = fn()

if getgenv().SkinHubLoaded then
	game:GetService("StarterGui"):SetCore("SendNotification", { Title = "Skin HUB v4.2", Text = "请勿重复执行", Duration = 3 })
	return
end

getgenv().SkinHubLoaded = true
local v2 = clock()
local v3 = v(game:GetService("HttpService"))
local v4 = v(game:GetService("TeleportService"))
local lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/deividcomsono/Obsidian/refs/heads/main/Library.lua"))()
loadstring(game:HttpGet("https://raw.githubusercontent.com/deividcomsono/Obsidian/refs/heads/main/addons/ThemeManager.lua"))()
loadstring(game:HttpGet("https://raw.githubusercontent.com/deividcomsono/Obsidian/refs/heads/main/addons/SaveManager.lua"))()
lib.ForceCheckbox = true
local options = lib.Options
local toggles = lib.Toggles
local tbl = {}
local lib2 = nil

pcall(function()
	lib2 = loadstring(game:HttpGet("https://raw.githubusercontent.com/mstudio45/MSESP/refs/heads/main/source.luau"))()
end)

if lib2 == nil then
	lib2 = getgenv().mstudio45_ESP
end

if lib2 ~= nil then
	pcall(function()
		lib2.GlobalConfig.Billboards = true
		lib2.GlobalConfig.Distance = true
	end)
end

tbl.addESP = function(arg)
	if lib2 == nil then
		return nil
	end
	local ok, result = pcall(lib2.Add, lib2, arg)
	if ok then
		return result
	end
	return nil
end

tbl.destroyESP = function(arg)
	if arg ~= nil then
		pcall(function()
			arg:Destroy()
		end)
	end
end

tbl.notify = function(arg, arg2)
	lib:Notify(arg, arg2 or 2)
end

local function fn2(arg, ...)
	local tbl2 = { ... }

	for i = 1, #tbl2 do
		local v5 = tbl2[i]

		if arg then
			arg = arg:FindFirstChild(v5)
		else
			arg = nil
		end

		if not arg then
			return nil
		end
	end

	return arg
end

local v5 = v(game:GetService("Players"))
local v6 = v(game:GetService("RunService"))
local v7 = v(game:GetService("UserInputService"))
local v8 = v(game:GetService("ReplicatedStorage"))
local v9 = v(game:GetService("TweenService"))
local v10 = v(game:GetService("Lighting"))
local v11 = v(game:GetService("Stats"))
local v12 = v(game:GetService("CoreGui"))
local v13 = v(game:GetService("MarketplaceService"))
local network = v11 and v11:FindFirstChild("Network")
network = network and network:FindFirstChild("ServerStatsItem")
local dataPing = network and network:FindFirstChild("Data Ping")
local localPlayer = v5.LocalPlayer

tbl.createFloatingButton = function(name, text, position, textSize, arg)
	local screenGui = Instance.new("ScreenGui")
	screenGui.Name = name
	screenGui.ResetOnSpawn = false
	screenGui.Parent = localPlayer:WaitForChild("PlayerGui")
	local textButton = Instance.new("TextButton")
	textButton.Size = UDim2.new(0, 60, 0, 60)
	textButton.Position = position
	textButton.BackgroundColor3 = lib.Scheme.MainColor
	textButton.BackgroundTransparency = 0.2
	textButton.BorderSizePixel = 0
	textButton.Text = text
	textButton.TextColor3 = Color3.new(1, 1, 1)
	textButton.TextSize = textSize
	textButton.Font = Enum.Font.GothamBold
	textButton.Parent = screenGui
	textButton.Active = true
	local uiCorner = Instance.new("UICorner")
	uiCorner.CornerRadius = UDim.new(0, 12)
	uiCorner.Parent = textButton
	local uiStroke = Instance.new("UIStroke")
	uiStroke.Color = lib.Scheme.AccentColor
	uiStroke.Thickness = 2.5
	uiStroke.Transparency = 0.3
	uiStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
	uiStroke.Parent = textButton
	local flag = false
	local flag2 = false
	local position2 = nil
	local position3 = nil
	local tbl2 = {}

	table.insert(tbl2, textButton.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 then
			flag = true
			flag2 = false
			position2 = input.Position
			position3 = textButton.Position
		end
	end))

	table.insert(tbl2, v7.InputChanged:Connect(function(input)
		if flag and input.UserInputType == Enum.UserInputType.MouseMovement then
			if (input.Position - position2).Magnitude > 4 then
				flag2 = true
			end

			local n = input.Position - position2
			textButton.Position = UDim2.new(position3.X.Scale, position3.X.Offset + n.X, position3.Y.Scale, position3.Y.Offset + n.Y)
		end
	end))

	table.insert(tbl2, v7.InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 then
			flag = false
		end
	end))

	table.insert(tbl2, screenGui.Destroying:Connect(function()
		for _, v14 in tbl2, nil, nil do
			pcall(function()
				v14:Disconnect()
			end)
		end
	end))

	textButton.MouseButton1Click:Connect(function()
		if flag2 then
			return
		end
		arg()
	end)

	return screenGui, textButton
end
