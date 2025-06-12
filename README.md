local function getMethodLoading()
	local GG = (getgenv and getgenv()) or _G or shared;
	if GG.IdenG ~= nil then
		Iden = GG.IdenG
	else
		pcall(function() Iden = identifyexecutor(); end);
	end;
	if game:GetService("CoreGui"):FindFirstChild("InputPcToMobile") then
		pcall(function() game:GetService("CoreGui"):FindFirstChild("InputPcToMobile"):Destroy(); end);
	end;
	if not game:GetService("CoreGui"):FindFirstChild("Neu") then
		local Neu = Instance.new("ScreenGui");
		Neu.Parent = game:GetService("CoreGui")
		Neu.IgnoreGuiInset = true;
		GG["Neu"] = Neu;
	end;

	GG["DisableSystem"] = true;

	local Configs = {
		["Stats"] = {
			["Melee"] = false;
			["Defense"] = false;
			["Sword"] = false;
			["Gun"] = false;
			["BloxFruit"] = false;
			["Points"] = 2
		};
		["Farming"] = {
			["Weapon"] = "Combat";
			["Level"] = {
				["AutoFarm"] = false;
				["AutoSea2"] = true;
				["AutoSea3"] = true;
				["TP_Speed"] = 2;
			};
			["Sea1"] = {
				["AutoSaber"] = false;
				["AutoSaberQuest"] = false;
				["Hop"] = false;
				["AutoSaw"] = false;
				["HopSaw"] = false;
			};
			["Sea2"] = {
				["AutoFactory"] = false;
				["AutoEctoplasm"] = false;
				["AutoBartilo"] = false;
			};
			["Sea3"] = {
				["AutoKillDoughKing"] = false;
				["AutoCastleRaid"] = false;
				["AutoElitehunter"] = false;
				["AutoFarmBone"] = false;
				["AutoSpawnCakePrince"] = false;
				["AutoCakePrince"] = false;
				["AutoFarmCakePrince"] = false;
				["AutoBuddySword"] = false;
				["AutoCarvender"] = false;
				["AutoCarvenderHop"] = false;
				["ArenaTrainer"] = false;
			};
			["AllSea"] = {
				["AutoHopSea"] = true;
				["AutoHopServer"] = false;
				["Melee"] = {
					["DeathStep"] = false;
					["SharkmanKarate"] = false;
				};
			};
		};
		["SeaEvents"] = {
			["Target"] = {
				["Shark"] = false;
				["Terrorshark"] = false;
				["Piranha"] = false;
				["FishCrewMember"] = false;
			};
			["TP_Speed"] = 3.5;
		};
		["Players"] = {
			["Client"] = {
				["AntiAFK"] = true;
				["Float"] = false;
				["Noclip"] = false;
				["FlySpeed"] = 1;
				["Fly"] = false;
				["DMGAura"] = false;
				["InfiniteEnergy"] = false;
				["AutoBusoHaki"] = true;
				["AutoOBVHaki"] = false;
				["WalkOnWater"] = false;
				["WalkSpeed"] = 1;
				["WalkSpeedToggle"] = false;
				["JumpPower"] = 50;
				["JumpPowerToggle"] = false;
				["MaxFPSCap"] = 60;
				["NoRender"] = false;
				["BlackScreen"] = false;
			};
			["Combat"] = {
				["Target"] = nil;
				["Spectate"] = false;
				["AutoKillTarget"] = false;
				["Aimbot"] = false;
				["SilentAim"] = false;
				["AutoBounty"] = false;
			};
			["Visual"] = {
				["HideDamageText"] = false;
				["HideNotifications"] = false;
				["HideChat"] = false;
				["HideLeaderboard"] = false;
			};
		};
		["Raid"] = {
			["Manual"] = {
				["SelectRaid"] = "Dark";
				["KillAura"] = false;
				["TeleportToNextIsland"] = false;
			};
		};
		["Teleport"] = {
			["Island"] = {
				["Target"] = nil;
			};
			["NPCs"] = {
				["Target"] = nil;
			};
			["Player"] = {
				["Target"] = nil;
			};
			["Mob_Boss"] = {
				["Target"] = nil;
			};
			["TP_Speed"] = 2;
			["TP_Bypass"] = false;
		};
		["Shop"] = {
			["AutoBuyLegendarySword"] = false;
			["AutoBuyHakiColor"] = false;
		};
		["ESP"] = {
			["Flower"] = false;
			["Fruits"] = false;
			["Players"] = false;
			["Chests"] = false;
			["IslandESP"] = false;
			["Mirage"] = false;
			["AdvancedFD"] = false;
			["RealFruits"] = false;
			["Gear"] = false;
			["S3W2"] = false; -- Don't touch if you don't know what is it
		};
		["Server"] = {
			["AutoExec"] = true;
		};
		["Settings"] = {
			["UI"] = {
				["Fly"] = {
					["Show_Hide"] = false;
					["Up_Down"] = false;
					["Big_Small"] = false;
				};
			};
			["System"] = {
				["ClientDebug"] = false;
				["Commands"] = false;
			};
			["AutoFarm"] = {
				["Hitbox"] = false;
				["BringMob"] = false;
				["BringMobType"] = "Target";
				["BringMobDistance"] = 180;
				["Stackable"] = true; -- ALWAYS ON ⚠️⚠️⚠️
				["PosX"] = 0;
				["PosY"] = 19;
				["PosZ"] = -10;
				["TP_Speed"] = 2;
			};
			["AutoRaid"] = {
				["PosX"] = 0;
				["PosY"] = 0;
				["PosZ"] = 0;
				["TP_Speed"] = 2;
			};
			["AutoBounty"] = {
				["PosX"] = 0;
				["PosY"] = 19;
				["PosZ"] = -10;
				["TP_Speed"] = 2;
			};
			["AutoChest"] = {
				["Detail"] = true;
				["Normal"] = true;
				["Fragment"] = true;
				["TP_Speed"] = 2
			};
			["Game"] = {
				["Gravity"] = 192;
				["Lighting_Tech"] = "ShadowMap";
			};
		};
		["System"] = {
			["WebhookLive"] = "";
			["WebhookRequest"] = "";
			["BREAKALLTHISSHITHAHAHAHAHA"] = false;
			["Selected_Boss"] = {
				["Obj"] = nil; ["Auto"] = false;
			};
			["Selected_Mob"] = {
				["Obj"] = nil; ["Auto"] = false;
			};
			["Selected_Material"] = {
				["Obj"] = nil; ["Auto"] = false;
			};
			["Astaootslai"] = false;
			["Backpack"] = {};
			["AutoRaid"] = false;
			["AutoChest"] = false;
			["SelectedShip"] = nil;
		};
	};

	local PData = {
		["Melee"] = {
			["BlackLeg"]= {
				["Have"] = false;
				["Level"] = 0;
			};
			["DeathStep"] = {
				["Have"] = false;
				["Level"] = 0;
				["Step2"] = false;
			};
			["FishmanKarate"] = {
				["Have"] = false;
				["Level"] = 0;
			};
			["SharkmanKarate"] = {
				["Have"] = false;
				["Level"] = 0;
				["Step2"] = false;
			};
		};
		["DidBartilo"] = false;
	};

	local VirtualInputManager = game:GetService('VirtualInputManager');
	local W = game:GetService("Workspace");
	local P = game:GetService("Players");
	local L = game:GetService("Lighting");
	local C = game:GetService("CoreGui");
	local H = game:GetService("RunService");
	local R = game:GetService("ReplicatedStorage");
	local VU = game:GetService("VirtualUser");
	local T = game:GetService("TeleportService");
	local CommF = R.Remotes.CommF_;
	local selff = P.LocalPlayer;
	local PSG = selff:WaitForChild("PlayerGui");
	local Data = selff.Data;
	local NPCs = W:WaitForChild("NPCs", 10);
	local Enemy = W:WaitForChild("Enemies", 10);
	local Backpack = selff.Backpack;
	local Neu = GG["Neu"];
	local Enem = {};
	local Chars = {};
	local CollectionService = game:GetService("CollectionService");
	local selc = selff.Character;
	local Cam = W:WaitForChild("Camera");
	local IgnoreE = false;
	if GG and GG.ActiveTime == nil then
		GG.ActiveTime = 0;
	end;
	if GG.FullAutoAzureEmber == nil then
		GG.FullAutoAzureEmber = true;
	end;
	Lib = nil;

	setc = setclipboard or toclipboard;
	tos = tostring;
	tablein = table.insert;
	tablecl = table.clear;
	print = print;
	tk = task;
	tspawn = tk.spawn;
	twait = tk.wait;
	tdelay = task.delay;
	wait = wait;
	FindFirstChild = game.FindFirstChild;
	Vec3 = Vector3.new;
	CF = CFrame.new;
	str = string;
	strgsub = str.gsub;
	strfind = str.find;
	pir = pairs;
	IsA = game.IsA;
	pcal = pcall;
	FindFirstChildOfClass = game.FindFirstChildOfClass;
	GetAttribute = game.GetAttribute;
	SetAttribute = game.SetAttribute;
	Instancen = Instance.new;
	tick = tick;
	delay = delay;
	SetPrimaryPartCFrame = W.SetPrimaryPartCFrame;
	GetPivot = W.GetPivot;
	GetChildren = game.GetChildren;
	GetDescendants = game.GetDescendants;
	FindFirstChildWhichIsA = game.FindFirstChildWhichIsA;
	mfloor = math.floor

	local NothingLibrary = loadstring(game:HttpGet('https://raw.githubusercontent.com/3345-c-a-t-s-u-s/neuron/refs/heads/main/script.lua'))();
	local Notification = NothingLibrary.Notification();

	local CONTROL = {F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0};

	FLYING = false;

	speedofthevfly = 1;

	speedofthefly = 1;

	local cmdm = selff:GetMouse();

	local screenGui2 = Instancen("ScreenGui"); screenGui2.Parent = game:GetService("CoreGui"); screenGui2.Name = "InputPcToMobile"; local buttonSize = UDim2.new(0.05, 0, 0.05, 0);

	local function createButton(key, position) local button = Instancen("TextButton"); button.Parent = screenGui2;
		button.Name = key; button.Text = key; button.Size = buttonSize; button.Position = position; button.BackgroundColor3 = Color3.new(0.5, 0.5, 0.5); local isPressed = false;
		button.MouseButton1Down:Connect(function() isPressed = true; if key == "W" then CONTROL.F = speedofthefly; elseif key == "S" then CONTROL.B = -speedofthefly; elseif key == "A" then CONTROL.L = -speedofthefly; elseif key == "D" then CONTROL.R = speedofthefly end; end);
		button.MouseButton1Up:Connect(function() isPressed = false; if key == "W" then CONTROL.F = 0; elseif key == "S" then CONTROL.B = 0; elseif key == "A" then CONTROL.L = 0; elseif key == "D" then CONTROL.R = 0; end; end);
	end;

	createButton("W", UDim2.new(0.05, 0, 0.1, 0));createButton("A", UDim2.new(0, 0, 0.2, 0));createButton("S", UDim2.new(0.05, 0, 0.3, 0));createButton("D", UDim2.new(0.1, 0, 0.2, 0));
	TTJYMobileIn = game:GetService("CoreGui"):WaitForChild("InputPcToMobile");

	JKLL, JKLLJ = pcall(function() require(R.Util.CameraShaker):Stop() end);

	placeId = game.PlaceId
	WorldCheck = {["First Sea"]=false,["Second Sea"]=false,["Third Sea"]=false}
	if placeId == 2753915549 then
		WorldCheck["First Sea"] = true;
	elseif placeId == 4442272183 then
		WorldCheck["Second Sea"] = true;
	elseif placeId == 7449423635 then
		WorldCheck["Third Sea"] = true;
	end;

	GG["DisableSystem"] = false;

	if WorldCheck["First Sea"] then
		BossList = {
			"The Gorilla King",
			"Bobby",
			"Yeti",
			"Mob Leader",
			"Vice Admiral",
			"Warden",
			"Chief Warden",
			"Swan",
			"Magma Admiral",
			"Fishman Lord",
			"Wysper",
			"Thunder God",
			"Cyborg",
			"Saber Expert"
		};
	elseif WorldCheck["Second Sea"] then
		BossList = {
			"Diamond",
			"Jeremy",
			"Fajita",
			"Don Swan",
			"Smoke Admiral",
			"Cursed Captain",
			"Darkbeard",
			"Order",
			"Awakened Ice Admiral",
			"Tide Keeper"
		};
	elseif WorldCheck["Third Sea"] then
		BossList = {
			"Stone",
			"Island Empress",
			"Kilo Admiral",
			"Captain Elephant",
			"Beautiful Pirate",
			"rip_indra True Form",
			"Longma",
			"Soul Reaper",
			"Cake Queen"
		};
	end;

	if WorldCheck["First Sea"] then
		MobList = {
			"Bandit",
			"Monkey",
			"Gorilla",
			"Pirate",
			"Brute",
			"Desert Bandit",
			"Desert Officer",
			"Snow Bandit",
			"Snowman",
			"Chief Petty Officer",
			"Sky Bandit",
			"Dark Master",
			"Toga Warrior",
			"Gladiator",
			"Military Soldier",
			"Military Spy",
			"Fishman Warrior",
			"Fishman Commando",
			"God's Guard",
			"Shanda",
			"Royal Squad",
			"Royal Soldier",
			"Galley Pirate",
			"Galley Captain"
		};
	elseif WorldCheck["Second Sea"] then
		MobList = {
			"Raider",
			"Mercenary",
			"Swan Pirate",
			"Factory Staff",
			"Marine Lieutenant",
			"Marine Captain",
			"Zombie",
			"Vampire",
			"Snow Trooper",
			"Winter Warrior",
			"Lab Subordinate",
			"Horned Warrior",
			"Magma Ninja",
			"Lava Pirate",
			"Ship Deckhand",
			"Ship Engineer",
			"Ship Steward",
			"Ship Officer",
			"Arctic Warrior",
			"Snow Lurker",
			"Sea Soldier",
			"Water Fighter"
		};
	elseif WorldCheck["Third Sea"] then
		MobList = {
			"Pirate Millionaire",
			"Dragon Crew Warrior",
			"Dragon Crew Archer",
			"Female Islander",
			"Giant Islander",
			"Marine Commodore",
			"Marine Rear Admiral",
			"Fishman Raider",
			"Fishman Captain",
			"Forest Pirate",
			"Mythological Pirate",
			"Jungle Pirate",
			"Musketeer Pirate",
			"Reborn Skeleton",
			"Living Zombie",
			"Demonic Soul",
			"Posessed Mummy",
			"Peanut Scout",
			"Peanut President",
			"Ice Cream Chef",
			"Ice Cream Commander",
			"Cookie Crafter",
			"Cake Guard",
			"Baking Staff",
			"Head Baker",
			"Cocoa Warrior",
			"Chocolate Bar Battler",
			"Sweet Thief",
			"Candy Rebel",
			"Candy Pirate",
			"Snow Demon",
			"Isle Outlaw",
			"Island Boy",
			"Sun-kissed Warrior",
			"Isle Champion"
		};
	end;

	function SendWeb(a, b)
		request({
			Url = a,
			Headers = {['Content-Type'] = 'application/json',},
			Method = 'POST',
			Body = game:GetService('HttpService'):JSONEncode({
				content = b;
			});
		});
	end;function tableTostring(t, indent)
		indent = indent or 0;
		local str = "{\n";
		local indentStr = string.rep("    ", indent);

		for key, value in pairs(t) do
			local keyStr = "";
			if type(key) == "string" then
				keyStr = '["' .. key .. '"] = ';
			elseif type(key) == "number" then
				keyStr = "[" .. key .. "] = ";
			end;

			if type(value) == "table" then
				str = str .. indentStr .. keyStr .. tableTostring(value, indent + 1) .. ",\n";
			else
				str = str .. indentStr .. keyStr .. tostring(value) .. ",\n";
			end;
		end;

		str = str .. string.rep("    ", indent - 1) .. "}";
		return str;
	end;

	function AutoExec()

	end;

	function RepText(obj, num, text)
		local newText = strgsub(text, ('.'):rep(num), function(a)
			return a;
		end);
		local awdadaAA = obj:NewTitle(newText)
		return awdadaAA;
	end;

	function dist(position)
		return selff:DistanceFromCharacter(position);
	end;function Tp2(xyz)
		if FindFirstChild(selff.Character, "HumanoidRootPart") then
			selff.Character.HumanoidRootPart.CFrame = xyz;
		end;
	end;function Tp(a, b, c, speedoftpNTP, obj)
		KLLOP = selff.Character.HumanoidRootPart;
		pKLLOP = KLLOP.Position;
		cpKLLOP = Vec3(pKLLOP.x, pKLLOP.y, pKLLOP.z);
		tpKLOOP, sdKLOOp, saveAlKLOOP, svetarKLLOP = nil
		if typeof(a) == "number" then
			tpKLOOP = Vec3(a, b, c); sdKLOOp = speedoftpNTP;
			saveAlKLOOP = CF(a, b, c);
			svetarKLLOP = CF(a, b, c);
		elseif typeof(a) == "CFrame" then
			tpKLOOP = a.Position; sdKLOOp = b;
			saveAlKLOOP = a
			svetarKLLOP = a
		end;
		typeofValidInstances = nil;
		if typeof(c) == "Instance" then
			FollowValidInstances = true;
			typeofValidInstances = c;
		else
			if typeof(obj) == "Instance" then
				FollowValidInstances = true;
				typeofValidInstances = obj;
			else
				FollowValidInstances = false;
			end;
		end;
		dtnKLOOP = (tpKLOOP - cpKLLOP).Unit;
		dceKLLOP = (tpKLOOP - cpKLLOP).Magnitude;
		ssKLLOP = mfloor(dceKLLOP / sdKLOOp);
		if not FindFirstChild(selff.Character, "Humanoid") then
			repeat twait(1); until FindFirstChild(selff.Character, "Humanoid") and FindFirstChild(selff.Character, "HumanoidRootPart") twait(1);
			KLLOP = selff.Character.HumanoidRootPart
			pKLLOP = KLLOP.Position;
		end;
		if FindFirstChild(selff.Character, "Humanoid") and selff.Character.Humanoid.Health <= 0 then
			repeat twait(1); until FindFirstChild(selff.Character, "Humanoid") and FindFirstChild(selff.Character, "HumanoidRootPart") and selff.Character.Humanoid.Health > 0 twait(1);
			KLLOP = selff.Character.HumanoidRootPart
			pKLLOP = KLLOP.Position;
		end;
		if not FollowValidInstances then
			for i = 1, ssKLLOP do
				if Configs.System.BREAKALLTHISSHITHAHAHAHAHA then break; end;
				if dist(saveAlKLOOP.Position) <= 50 then Tp2(svetarKLLOP); break; end;
				cpKLLOP = cpKLLOP + dtnKLOOP * sdKLOOp;
				selff.Character.HumanoidRootPart.CFrame = CF(cpKLLOP);
				twait();
			end;
		else
			pcal(function()
				for i = 1, ssKLLOP do
					tpKLOOP = typeofValidInstances.Position;
					dtnKLOOP = (tpKLOOP - cpKLLOP).Unit;
					dceKLLOP = (tpKLOOP - cpKLLOP).Magnitude;
					ssKLLOP = mfloor(dceKLLOP / sdKLOOp);
					cpKLLOP = cpKLLOP + dtnKLOOP * sdKLOOp;
					if Configs.System.BREAKALLTHISSHITHAHAHAHAHA then break; end;
					if dist(typeofValidInstances.Position) <= 50 then Tp2(tpKLOOP); break; end;
					selff.Character.HumanoidRootPart.CFrame = CF(cpKLLOP);
					twait();
				end;
			end);
		end;
	end;function TpBypass(a)
		if Configs.Teleport.TP_Bypass then
			if typeof(a) == "CFrame" and selc then
				SetPrimaryPartCFrame(selc, a)
				wait();
				selc.Humanoid.Health = 0;
				repeat twait() until selc and selc.Humanoid
				SetPrimaryPartCFrame(selc, a)
				SetPrimaryPartCFrame(selc, a)
				SetPrimaryPartCFrame(selc, a)
			end;
		end; wait(0.7)
		repeat twait(1); until FindFirstChild(selff.Character, "Humanoid") and selff.Character.Humanoid.Health > 0
	end;function tpwithseat(xyz,speedoftpNTP)
		guyejrigrjerhjfcnwhfwefji = selff.Character.HumanoidRootPart;
		reuifrefiremfvuhuevr = guyejrigrjerhjfcnwhfwefji.Position;
		HHAHDAWUDnenfsfewuhfefwfowife = Vec3(reuifrefiremfvuhuevr.x, reuifrefiremfvuhuevr.y, reuifrefiremfvuhuevr.z);
		SDFGHJKWDuewuewfjewjfuew = xyz.Position;
	
		HDNwajdiir3jiwisjdjifew = (SDFGHJKWDuewuewfjewjfuew - HHAHDAWUDnenfsfewuhfefwfowife).Unit;
		wjjdjiwjidwjiwejiewifjwijweifoj = (SDFGHJKWDuewuewfjewjfuew - HHAHDAWUDnenfsfewuhfefwfowife).Magnitude;
		ajakdapujupyjlyljyujyupjpuy = mfloor(wjjdjiwjidwjiwejiewifjwijweifoj / speedoftpNTP);
		if not FindFirstChild(selff.Character, "Humanoid") then
			repeat twait(1.867); until FindFirstChild(selff.Character, "Humanoid")
		end;
		for i = 1, ajakdapujupyjlyljyujyupjpuy do
			if Configs.System.BREAKALLTHISSHITHAHAHAHAHA then break; end;
			HHAHDAWUDnenfsfewuhfefwfowife = HHAHDAWUDnenfsfewuhfefwfowife + HDNwajdiir3jiwisjdjifew * speedoftpNTP;
			selff.Character.Humanoid.SeatPart.Parent:SetPrimaryPartCFrame(CF(HHAHDAWUDnenfsfewuhfefwfowife));
			twait();
		end;
	end;function BP()
		Configs.System.Backpack = Backpack:GetChildren();
		return Backpack:GetChildren();
	end;function SetHum(obj)
		obj.HumanoidRootPart.CanCollide = false; obj.Humanoid.WalkSpeed = 0; obj.Head.CanCollide = false;
		obj.HumanoidRootPart.CanQuery = false;
		if Configs.Settings.AutoFarm.Hitbox then
			obj.HumanoidRootPart.Size = Vec3(64, 64, 64);
		end;
		if FindFirstChild(obj, "Humanoid") then
			obj.Humanoid.WalkSpeed = 0;
			if FindFirstChild(obj.Humanoid, "Animator") then
				obj.Humanoid.Animator:Destroy();
			end;
		end;
		if FindFirstChild(obj, "Busy") then
			obj.Busy.Value = true;
		end;
		if FindFirstChild(obj, "Stun") then
			obj.Stun.Value = 1;
		end;
	end;function getInventory(value, types)
		if FindFirstChild(Backpack, value) then
			if types == "Check" then return true; end;
		elseif FindFirstChild(selff.Character, value) then
			if types == "Check" then return true; end;
		else
			for i, v in pairs(CommF:InvokeServer("getInventory")) do
				if type(v) == "table" then
					if types == "Check" and value == v.Name then
						return true
					elseif types == "Count" then
						return v.Count
					elseif types == "MaxCount" then
						return v.MaxCount
					end;
				end;
			end;
		end;
		return false;
	end;

	function EquipWeapon(Name)
		if FindFirstChild(Backpack, Name) then
			ToolJKNLL = FindFirstChild(Backpack, Name);
			twait(0.02);
			selc.Humanoid:EquipTool(ToolJKNLL);
			twait(0.5);
		end;
	end;

	function InfStamina()
		if selc and selc.Parent then
			selc.Energy.Value = selc.Energy.MaxValue;
		end;
	end;

	function AutoChest(bool)
		ProcessAutoChest = bool;
		if Configs.Settings.AutoChest.Normal and not Configs.Settings.AutoChest.Detail then
			shdtts = math.huge; nearestChest = nil;
			for _, v in pir(W:GetChildren()) do
				if IsA(v, "BasePart") and (v.Name == "Chest1" or v.Name == "Chest2" or v.Name == "Chest3" or v.Name == "FragChest") then
					if dist(v.Position) < shdtts then 
						nearestChest = v; shdtts = dist(v.Position) 
					end; 
				end; 
			end;
		elseif (Configs.Settings.AutoChest.Normal and Configs.Settings.AutoChest.Detail) or (not Configs.Settings.AutoChest.Normal and Configs.Settings.AutoChest.Detail) then
			shdtts = math.huge; nearestChest = nil;
			for _, v in pir(W:GetDescendants()) do
				if IsA(v, "BasePart") and (v.Name == "Chest1" or v.Name == "Chest2" or v.Name == "Chest3" or v.Name == "FragChest") then
					if dist(v.Position) < shdtts then
						nearestChest = v; shdtts = dist(v.Position); 
					end; 
				end; 
			end; 
		end;
		if nearestChest ~= nil then
			Tp(nearestChest.Position.X, nearestChest.Position.Y, nearestChest.Position.Z, Configs.Settings.AutoChest.TP_Speed); twait();
			Tp2(CF(nearestChest.Position.X, nearestChest.Position.Y, nearestChest.Position.Z)); twait(1);
		end;
		ProcessAutoChest = false;
	end;

	DamageAura = true;
	Fast = false;
	NeedAttacking = false;
	if JKLL then
		getHits = function(Size)
			local Hits = {};
			if nearbymon then
				for i=1,#Enem do local v = Enem[i];
					local Human = FindFirstChildOfClass(v, "Humanoid");
					if Human and Human.RootPart and Human.Health > 0 and dist(Human.RootPart.Position) < Size+5 then
						tablein(Hits,Human.RootPart);
					end;
				end;
				for i=1,#Chars do local v = Chars[i];
					if v ~= selff.Character then
						local Human = FindFirstChildOfClass(v, "Humanoid");
						if Human and Human.RootPart and Human.Health > 0 and dist(Human.RootPart.Position) < Size+5 then
							tablein(Hits,Human.RootPart);
						end;
					end;
				end;
			end;
			return Hits;
		end;
		local CurrentAllMob = {};
		local canHits = {};
		require(R.Util.CameraShaker):Stop();
		PC = require(selff.PlayerScripts.CombatFramework.Particle);
		RL = require(R.CombatFramework.RigLib);
		DMG = require(selff.PlayerScripts.CombatFramework.Particle.Damage);
		RigC = getupvalue(require(selff.PlayerScripts.CombatFramework.RigController),2);
		Combat =  getupvalue(require(selff.PlayerScripts.CombatFramework),2);
		task.spawn(function()
			local stacking = 0;
			local printCooldown = 0;
			while twait(.075) do
				nearbymon = false;
				tablecl(CurrentAllMob);
				tablecl(canHits);
				local mobs = CollectionService:GetTagged("ActiveRig");
				for i=1,#mobs do local v = mobs[i];
					Human = FindFirstChildOfClass(v, "Humanoid");
					if Human and Human.Health > 0 and Human.RootPart and v ~= Char then
						local IsPlayer = P:GetPlayerFromCharacter(v);
						local IsAlly = IsPlayer and CollectionService:HasTag(IsPlayer,"Ally"..selff.Name);
						if not IsAlly then
							CurrentAllMob[#CurrentAllMob + 1] = v;
							if not nearbymon and (FindFirstChild(selff.Character, "HumanoidRootPart") and (v.HumanoidRootPart.Position - selff.Character.HumanoidRootPart.Position).Magnitude <= 65) then
								nearbymon = true;
							end;
						end;
					end;
				end;
				if nearbymon then
					local Players = P:GetPlayers()
					for i=1,#Enem do local v = Enem[i]
						local Human = FindFirstChildOfClass(v, "Humanoid");
						if Human and Human.RootPart and Human.Health > 0 and (FindFirstChild(selff.Character, "HumanoidRootPart") and (v.HumanoidRootPart.Position - selff.Character.HumanoidRootPart.Position).Magnitude <= 65) then
							canHits[#canHits+1] = Human.RootPart;
						end;
					end;
					for i=1,#Players do local v = Players[i].Character;
						if not GetAttribute(Players[i], "PvpDisabled") and v and v ~= selff.Character then
							local Human = FindFirstChildOfClass(v, "Humanoid");
							if Human and Human.RootPart and Human.Health > 0 and (FindFirstChild(selff.Character, "HumanoidRootPart") and (v.HumanoidRootPart.Position - selff.Character.HumanoidRootPart.Position).Magnitude <= 65) then
								canHits[#canHits+1] = Human.RootPart;
							end;
						end;
					end;
				end;
			end;
		end);
		local shared = getgenv();
		local Data = Combat;
		local Blank = function() end;
		local RigEvent = R.RigControllerEvent;
		local Validator = R.Remotes.Validator;
		local Animation = Instancen("Animation");
		local RecentlyFired = 0;
		local AttackCD = 0;
		local Controller;
		local lastFireValid = 0;
		local MaxLag = 350;
		fucker = 0.07;
		TryLag = 0;
		local resetCD = function()
			local WeaponName = Controller.currentWeaponModel.Name;
			local Cooldown = {
				combat = 0.07
			};
			AttackCD = tick() + (fucker and Cooldown[WeaponName:lower()] or fucker or 0.285) + ((TryLag/MaxLag)*0.3);
			RigEvent.FireServer(RigEvent,"weaponChange",WeaponName);
			TryLag = TryLag + 1;
			task.delay((fucker or 0.285) + (TryLag+0.5/MaxLag)*0.3,function()
				TryLag = TryLag - 1;
			end);
		end;
		if not shared.orl then shared.orl = RL.wrapAttackAnimationAsync end;
		if not shared.cpc then shared.cpc = PC.play end;
		if not shared.dnew then shared.dnew = DMG.new end;
		if not shared.attack then shared.attack = RigC.attack end;
		RL.wrapAttackAnimationAsync = function(a,b,c,d,func)
			if not getgenv().FastZure then
				PC.play = shared.cpc;
				return shared.orl(a,b,c,65,func);
			end;
			local Radius = (getgenv().FastZure and getgenv().FastZureRadius) or 65;
			if canHits and #canHits > 0 then
				PC.play = function() end;
				a:Play(0.01,0.01,0.01);
				func(canHits);
				twait(a.length * 0.5);
				a:Stop();
			end;
		end;
		task.spawn(function()
			local Data = Combat;
			local Blank = function() end;
			local RigEvent = R.RigControllerEvent;
			local Animation = Instancen("Animation");
			local RecentlyFired = 0;
			local AttackCD = 0;
			local Controller;
			local lastFireValid = 0;
			local MaxLag = 350;
			fucker = 0.07;
			TryLag = 0;
			local resetCD = function()
				local WeaponName = Controller.currentWeaponModel.Name;
				local Cooldown = {
					combat = 0.07;
				}
				AttackCD = tick() + (fucker and Cooldown[WeaponName:lower()] or fucker or 0.285) + ((TryLag/MaxLag)*0.3);
				RigEvent.FireServer(RigEvent,"weaponChange",WeaponName);
				TryLag = TryLag + 1;
				tdelay((fucker or 0.285) + (TryLag+0.5/MaxLag)*0.3,function()
					TryLag = TryLag - 1;
				end);
			end;
			if not shared.orl then shared.orl = RL.wrapAttackAnimationAsync end;
			if not shared.cpc then shared.cpc = PC.play end;
			if not shared.dnew then shared.dnew = DMG.new end;
			if not shared.attack then shared.attack = RigC.attack end;
			RL.wrapAttackAnimationAsync = function(a,b,c,d,func)
				if not NeedAttacking then
					PC.play = shared.cpc;
					return shared.orl(a,b,c,65,func);
				end;
				local Radius = (DamageAura and DamageAuraRadius) or 65;
				if canHits and #canHits > 0 then
					PC.play = function() end;
					a:Play(0.00075,0.01,0.01);
					func(canHits);
					twait(a.length * 0.5);
					a:Stop();
				end;
			end;
			while H.Stepped:Wait() do
				if #canHits > 0 then
					Controller = Data.activeController;
					if NormalClick then
						pcal(tspawn, Controller.attack,Controller);
					end;
					if Controller and Controller.equipped and Controller.currentWeaponModel then
						if (NeedAttacking and DamageAura) then
							if Fast and tick() > AttackCD and not DisableFastAttack then
								resetCD();
							end
							if tick() - lastFireValid > 0.5 or not Fast then
								Controller.timeToNextAttack = 0;
								Controller.hitboxMagnitude = 65;
								pcal(tspawn, Controller.attack,Controller);
								lastFireValid = tick();
							end;
							local AID3 = Controller.anims.basic[3];
							local AID2 = Controller.anims.basic[2];
							local ID = AID3 or AID2;
							Animation.AnimationId = ID;
							local Playing = Controller.humanoid:LoadAnimation(Animation);
							Playing:Play(0.00075,0.01,0.01);
							RigEvent.FireServer(RigEvent,"hit",canHits,AID3 and 3 or 2,"");
							-- AttackSignal:Fire();
							delay(.5,function()
								Playing:Stop();
							end);
						end;
					end;
				end;
			end;
		end);
	else
		--selff:Kick("Unsupported Executor .. We are trying our best to make this support every executor. Thank you for using TTJY Hub.");
		warn("Unsupported Executor: Attempt to require")
	end;

	local Hitc;
	local VMVM = game:GetService('VirtualInputManager');
	Hitc = function()
		if Iden == "Solara" or Iden == "Celery" then
			VU:CaptureController();
			VU:Button1Down(Vector2.new(9999, 9999));
		end;
	end;

	function keyPress(Key, Press)
		VirtualInputManager:SendKeyEvent(Press, Key, false, game);
	end;

	KUYHB = nil;
	KUYONC = nil;
	AnyWhereBypass = function(CFrame)
		if CommF:InvokeServer("BuyFishmanKarate") == 1 or CommF:InvokeServer("BuyFishmanKarate") == 2 then
			WhatKindWow = Configs.Farming.Weapon
			IgnoreE = true;
			Configs.Farming.Weapon = "Fishman Karate";
			if FindFirstChild(selff.Backpack, "Fishman Karate") then
				EquipWeapon("Fishman Karate"); twait();
				KUYTP = 0;
				selff.Character.HumanoidRootPart.CFrame = CFrame; wait(0.28007) keyPress(Enum.KeyCode.C, true); keyPress(Enum.KeyCode.C, false);

				--[[
				tspawn(function()
					repeat twait();
						KUYTP = KUYTP + 1;
					until KUYTP >= 100;
					if KUYHB ~= nil then
						KUYHB:Disconnect();
						KUYHB = nil;
						KUYONC:Disconnect();
						KUYONC = nil;
						selc.HumanoidRootPart.Anchored = false;
					end;
				end);
				KUYHB = H.Heartbeat:Connect(function()
					keyPress(Enum.KeyCode.C, true);
					selc.HumanoidRootPart.CFrame = CFrame;
					selc.HumanoidRootPart.Anchored = true;
					keyPress(Enum.KeyCode.C, false);
				end);
				KUYONC = selc.HumanoidRootPart:GetPropertyChangedSignal("CFrame"):Connect(function()
					selc.HumanoidRootPart.CFrame = CFrame;
				end);

				--]]
			else
				KUYTP = 0;
				tspawn(function()
					repeat twait();
						KUYTP = KUYTP + 1;
					until KUYTP >= 100;
					if KUYHB ~= nil then
						KUYHB:Disconnect();
						KUYHB = nil;
						KUYONC:Disconnect();
						KUYONC = nil;
						selc.HumanoidRootPart.Anchored = false;
					end;
				end);
				KUYHB = H.Heartbeat:Connect(function()
					keyPress(Enum.KeyCode.C, true);
					selc.HumanoidRootPart.CFrame = CFrame;
					selc.HumanoidRootPart.Anchored = true;
					keyPress(Enum.KeyCode.C, false);
				end);
				KUYONC = selc.HumanoidRootPart:GetPropertyChangedSignal("CFrame"):Connect(function()
					selc.HumanoidRootPart.CFrame = CFrame;
				end);
			end;
		end;
	end;

	function CheckLevel()
		Lv = selff.Data.Level.Value;
		if WorldCheck["First Sea"] then
			if ((Lv == 1 or Lv <= 9) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Bandit" and not Configs.Farming.Level.AutoFarm) then
				return {
					[1] = "Bandit [Lv. 5]";
					[2] = "BanditQuest1";
					[3] = 1;
					[4] = "Bandit";
					[5] = CF(1060.9383544922, 16.455066680908, 1547.7841796875);
					[6] = CF(1038.5533447266, 41.296249389648, 1576.5098876953)
				}
			elseif ((Lv == 10 or Lv <= 14) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Monkey") then
				return {
					[1] = "Monkey [Lv. 14]";
					[2] = "JungleQuest";
					[3] = 1;
					[4] = "Monkey";
					[5] = CF(-1601.6553955078, 36.85213470459, 153.38809204102);
					[6] = CF(-1448.1446533203, 50.851993560791, 63.60718536377)
				}
			elseif ((Lv == 15 or Lv <= 29) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Gorilla") then
				return {
					[1] = "Gorilla [Lv. 20]";
					[2] = "JungleQuest";
					[3] = 2;
					[4] = "Gorilla";
					[5] = CF(-1601.6553955078, 36.85213470459, 153.38809204102);
					[6] = CF(-1142.6488037109, 40.462348937988, -515.39227294922)
				}
			elseif ((Lv == 30 or Lv <= 39) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Pirate") then
				return {
					[1] = "Pirate [Lv. 35]";
					[2] = "BuggyQuest1";
					[3] = 1;
					[4] = "Pirate";
					[5] = CF(-1140.1761474609, 4.752049446106, 3827.4057617188);
					[6] = CF(-1201.0881347656, 40.628940582275, 3857.5966796875)
				}
			elseif ((Lv == 40 or Lv <= 59) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Brute") then 
				return {
					[1] = "Brute [Lv. 45]";
					[2] = "BuggyQuest1";
					[3] = 2;
					[4] = "Brute";
					[5] = CF(-1140.1761474609, 4.752049446106, 3827.4057617188);
					[6] = CF(-1387.5324707031, 24.592035293579, 4100.9575195313)
				}
			elseif ((Lv == 60 or Lv <= 74) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Desert Bandit") then 
				return {
					[1] = "Desert Bandit [Lv. 60]";
					[2] = "DesertQuest";
					[3] = 1;
					[4] = "Desert Bandit";
					[5] = CF(896.51721191406, 6.4384617805481, 4390.1494140625);
					[6] = CF(984.99896240234, 16.109552383423, 4417.91015625)
				}
			elseif ((Lv == 75 or Lv <= 89) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Desert Officer") then 
				return {
					[1] = "Desert Officer [Lv. 70]";
					[2] = "DesertQuest";
					[3] = 2;
					[4] = "Desert Officer";
					[5] = CF(896.51721191406, 6.4384617805481, 4390.1494140625);
					[6] = CF(1547.1510009766, 14.452038764954, 4381.8002929688)
				}
			elseif ((Lv == 90 or Lv <= 99) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Snow Bandit") then 
				return {
					[1] = "Snow Bandit [Lv. 90]";
					[2] = "SnowQuest";
					[3] = 1;
					[4] = "Snow Bandit";
					[5] = CF(1386.8073730469, 87.272789001465, -1298.3576660156);
					[6] = CF(1356.3028564453, 105.76865386963, -1328.2418212891)
				}
			elseif ((Lv == 100 or Lv <= 119) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Snowman") then 
				return {
					[1] = "Snowman [Lv. 100]";
					[2] = "SnowQuest";
					[3] = 2;
					[4] = "Snowman";
					[5] = CF(1386.8073730469, 87.272789001465, -1298.3576660156);
					[6] = CF(1218.7956542969, 138.01184082031, -1488.0262451172)
				}
			elseif ((Lv == 120 or Lv <= 149) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Chief Petty Officer") then 
				return {
					[1] = "Chief Petty Officer [Lv. 120]";
					[2] = "MarineQuest2";
					[3] = 1;
					[4] = "Chief Petty Officer";
					[5] = CF(-5035.49609375, 28.677835464478, 4324.1840820313);
					[6] = CF(-4931.1552734375, 65.793113708496, 4121.8393554688)
				}
			elseif ((Lv == 150 or Lv <= 174) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Sky Bandit") then 
				if Configs.Farming.Level.AutoFarm and (Vec3(-4842.1372070313, 717.69543457031, -2623.0483398438) - selc.HumanoidRootPart.Position).Magnitude > 3000 then
					CommF:InvokeServer("requestEntrance", Vec3(-4607.82275390625, 874.3905029296875, -1667.556884765625));
				end;
				return {
					[1] = "Sky Bandit [Lv. 150]";
					[2] = "SkyQuest";
					[3] = 1;
					[4] = "Sky Bandit";
					[5] = CF(-4842.1372070313, 717.69543457031, -2623.0483398438);
					[6] = CF(-4955.6411132813, 365.46365356445, -2908.1865234375)
				}         
			elseif ((Lv == 175 or Lv <= 189) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Dark Master") then 
				if Configs.Farming.Level.AutoFarm and (Vec3(-4842.1372070313, 717.69543457031, -2623.0483398438) - selff.Character.HumanoidRootPart.Position).Magnitude > 3000 then
					CommF:InvokeServer("requestEntrance", Vec3(-4607.82275390625, 874.3905029296875, -1667.556884765625));
				end;
				return {
					[1] = "Dark Master [Lv. 175]";
					[2] = "SkyQuest";
					[3] = 2;
					[4] = "Dark Master";
					[5] = CF(-4842.1372070313, 717.69543457031, -2623.0483398438);
					[6] = CF(-5357.3515625, 449.032958984375, -2265.667236328125)
				}
			elseif ((Lv == 190 or Lv <= 209) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Prisoner") then 
				return {
					[1] = "Prisoner [Lv. 190]";
					[2] = "PrisonerQuest";
					[3] = 1;
					[4] = "Prisoner";
					[5] = CF(5308.98828125, 1.7804901599884033, 475.06048583984375);
					[6] = CF(5164.1083984375, 15.778244972229004, 489.6533508300781)
				}
			elseif ((Lv == 210 or Lv <= 249) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Dangerous Prisoner") then 
				return {
					[1] = "Dangerous Prisoner [Lv. 210]";
					[2] = "PrisonerQuest";
					[3] = 2;
					[4] = "Dangerous Prisoner";
					[5] = CF(5308.98828125, 1.7804901599884033, 475.06048583984375);
					[6] = CF(5564.0322265625, 15.759016036987305, 682.16455078125)
				}
			elseif ((Lv == 250 or Lv <= 274) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Toga Warrior") then 
				return {
					[1] = "Toga Warrior [Lv. 250]";
					[2] = "ColosseumQuest";
					[3] = 1;
					[4] = "Toga Warrior";
					[5] = CF(-1577.7890625, 7.4151420593262, -2984.4838867188);
					[6] = CF(-1872.5166015625, 49.080215454102, -2913.810546875)
				}
			elseif ((Lv == 275 or Lv <= 299) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Gladiator") then 
				return {
					[1] = "Gladiator [Lv. 275]";
					[2] = "ColosseumQuest";
					[3] = 2;
					[4] = "Gladiator";
					[5] = CF(-1577.7890625, 7.4151420593262, -2984.4838867188);
					[6] = CF(-1324.32666015625, 58.49076843261719, -3242.227294921875)
				}
			elseif ((Lv == 300 or Lv <= 329) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Military Soldier") then 
				return {
					[1] = "Military Soldier [Lv. 300]";
					[2] = "MagmaQuest";
					[3] = 1;
					[4] = "Military Soldier";
					[5] = CF(-5316.1157226563, 12.262831687927, 8517.00390625);
					[6] = CF(-5369.0004882813, 61.24352645874, 8556.4921875)
				}
			elseif ((Lv == 330 or Lv <= 374) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Military Spy") then 
				return {
					[1] = "Military Spy [Lv. 325]";
					[2] = "MagmaQuest";
					[3] = 2;
					[4] = "Military Spy";
					[5] = CF(-5316.1157226563, 12.262831687927, 8517.00390625);
					[6] = CF(-5984.0532226563, 82.14656829834, 8753.326171875)
				}
			elseif ((Lv == 375 or Lv <= 399) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Fishman Warrior") then 
				if Configs.Farming.Level.AutoFarm and (Vec3(61122.65234375, 18.497442245483, 1569.3997802734) - selff.Character.HumanoidRootPart.Position).Magnitude > 3000 then
					CommF:InvokeServer("requestEntrance",Vec3(61163.8515625, 11.6796875, 1819.7841796875));
				end;
				return {
					[1] = "Fishman Warrior [Lv. 375]";
					[2] = "FishmanQuest";
					[3] = 1;
					[4] = "Fishman Warrior";
					[5] = CF(61122.65234375, 18.497442245483, 1569.3997802734);
					[6] = CF(60844.10546875, 98.462875366211, 1298.3985595703)
				}
			elseif ((Lv == 400 or Lv <= 449) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Fishman Commando") then 
				
				if Configs.Farming.Level.AutoFarm and (Vec3(61122.65234375, 18.497442245483, 1569.3997802734) - selff.Character.HumanoidRootPart.Position).Magnitude > 3000 then
					CommF:InvokeServer("requestEntrance",Vec3(61163.8515625, 11.6796875, 1819.7841796875));
				end;
				return {
					[1] = "Fishman Commando [Lv. 400]";
					[2] = "FishmanQuest";
					[3] = 2;
					[4] = "Fishman Commando";
					[5] = CF(61122.65234375, 18.497442245483, 1569.3997802734);
					[6] = CF(61738.3984375, 64.207321166992, 1433.8375244141)
				}
			elseif ((Lv == 450 or Lv <= 474) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "God's Guard") then 
				if Configs.Farming.Level.AutoFarm and (Vec3(-4721.8603515625, 845.30297851563, -1953.8489990234) - selff.Character.HumanoidRootPart.Position).Magnitude > 3000 then
					CommF:InvokeServer("requestEntrance",Vec3(-4607.82275, 872.54248, -1667.55688));
				end;
				return {
					[1] = "God's Guard [Lv. 450]";
					[2] = "SkyExp1Quest";
					[3] = 1;
					[4] = "God's Guard";
					[5] = CF(-4721.8603515625, 845.30297851563, -1953.8489990234);
					[6] = CF(-4628.0498046875, 866.92877197266, -1931.2352294922)
				}
			elseif ((Lv == 475 or Lv <= 524) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Shanda") then 
				if Configs.Farming.Level.AutoFarm and (Vec3(-7863.1596679688, 5545.5190429688, -378.42266845703) - selff.Character.HumanoidRootPart.Position).Magnitude > 3000 then
					CommF:InvokeServer("requestEntrance", Vec3(-7894.6176757813, 5547.1416015625, -380.29119873047)); wait(3);
				end;
				return {
					[1] = "Shanda [Lv. 475]";
					[2] = "SkyExp1Quest";
					[3] = 2;
					[4] = "Shanda";
					[5] = CF(-7863.1596679688, 5545.5190429688, -378.42266845703);
					[6] = CF(-7685.1474609375, 5601.0751953125, -441.38876342773)
				}
			elseif ((Lv == 525 or Lv <= 549) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Royal Squad") then 
				return {
					[1] = "Royal Squad [Lv. 525]";
					[2] = "SkyExp2Quest";
					[3] = 1;
					[4] = "Royal Squad";
					[5] = CF(-7903.3828125, 5635.9897460938, -1410.923828125);
					[6] = CF(-7654.2514648438, 5637.1079101563, -1407.7550048828)
				}
			elseif ((Lv == 550 or Lv <= 624) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Royal Soldier") then 
				return {
					[1] = "Royal Soldier [Lv. 550]";
					[2] = "SkyExp2Quest";
					[3] = 2;
					[4] = "Royal Soldier";
					[5] = CF(-7903.3828125, 5635.9897460938, -1410.923828125);
					[6] = CF(-7760.4106445313, 5679.9077148438, -1884.8112792969)
				}
			elseif ((Lv == 625 or Lv <= 649) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Galley Pirate") then 
				return {
					[1] = "Galley Pirate [Lv. 625]";
					[2] = "FountainQuest";
					[3] = 1;
					[4] = "Galley Pirate";
					[5] = CF(5258.2788085938, 38.526931762695, 4050.044921875);
					[6] = CF(5557.1684570313, 152.32717895508, 3998.7758789063)
				}
			elseif (Lv >= 650 and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Galley Captain") then 
				return {
					[1] = "Galley Captain [Lv. 650]";
					[2] = "FountainQuest";
					[3] = 2;
					[4] = "Galley Captain";
					[5] = CF(5258.2788085938, 38.526931762695, 4050.044921875);
					[6] = CF(5677.6772460938, 92.786109924316, 4966.6323242188)
				}
			end
		elseif WorldCheck["Second Sea"] then
			if ((Lv == 700 or Lv <= 724) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Raider") then 
				return {
					[1] = "Raider [Lv. 700]";
					[2] = "Area1Quest";
					[3] = 1;
					[4] = "Raider";
					[5] = CF(-427.72567749023, 72.99634552002, 1835.9426269531);
					[6] = CF(68.874565124512, 93.635643005371, 2429.6752929688)
				}
			elseif ((Lv == 725 or Lv <= 774) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Mercenary") then 
				return {
					[1] = "Mercenary [Lv. 725]";
					[2] = "Area1Quest";
					[3] = 2;
					[4] = "Mercenary";
					[5] = CF(-427.72567749023, 72.99634552002, 1835.9426269531);
					[6] = CF(-864.85009765625, 122.47104644775, 1453.1505126953)
				}
			elseif ((Lv == 775 or Lv <= 799) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Swan Pirate") then 
				return {
					[1] = "Swan Pirate [Lv. 775]";
					[2] = "Area2Quest";
					[3] = 1;
					[4] = "Swan Pirate";
					[5] = CF(635.61151123047, 73.096351623535, 917.81298828125);
					[6] = CF(1065.3669433594, 137.64012145996, 1324.3798828125)
				}
			elseif ((Lv == 800 or Lv <= 874) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Factory Staff") then 
				return {
					[1] = "Factory Staff [Lv. 800]";
					[2] = "Area2Quest";
					[3] = 2;
					[4] = "Factory Staff";
					[5] = CF(635.61151123047, 73.096351623535, 917.81298828125);
					[6] = CF(533.22045898438, 128.46876525879, 355.62615966797)
				}
			elseif ((Lv == 875 or Lv <= 899) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Marine Lieutenant") then 
				return {
					[1] = "Marine Lieutenant [Lv. 875]";
					[2] = "MarineQuest3";
					[3] = 1;
					[4] = "Marine Lieutenant";
					[5] = CF(-2440.9934082031, 73.04190826416, -3217.7082519531);
					[6] = CF(-2489.2622070313, 84.613594055176, -3151.8830566406)
				}
			elseif ((Lv == 900 or Lv <= 949) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Marine Captain") then 
				return {
					[1] = "Marine Captain [Lv. 900]";
					[2] = "MarineQuest3";
					[3] = 2;
					[4] = "Marine Captain";
					[5] = CF(-2440.9934082031, 73.04190826416, -3217.7082519531);
					[6] = CF(-2335.2026367188, 79.786659240723, -3245.8674316406)
				}
			elseif ((Lv == 950 or Lv <= 974) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Zombie") then 
				return {
					[1] = "Zombie [Lv. 950]";
					[2] = "ZombieQuest";
					[3] = 1;
					[4] = "Zombie";
					[5] = CF(-5494.3413085938, 48.505931854248, -794.59094238281);
					[6] = CF(-5536.4970703125, 101.08577728271, -835.59075927734)
				}
			elseif ((Lv == 975 or Lv <= 999) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Vampire") then 
				return {
					[1] = "Vampire [Lv. 975]";
					[2] = "ZombieQuest";
					[3] = 2;
					[4] = "Vampire";
					[5] = CF(-5494.3413085938, 48.505931854248, -794.59094238281);
					[6] = CF(-5806.1098632813, 16.722528457642, -1164.4384765625)
				}
			elseif ((Lv == 1000 or Lv <= 1049) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Snow Trooper") then 
				return {
					[1] = "Snow Trooper [Lv. 1000]";
					[2] = "SnowMountainQuest";
					[3] = 1;
					[4] = "Snow Trooper";
					[5] = CF(607.05963134766, 401.44781494141, -5370.5546875);
					[6] = CF(535.21051025391, 432.74209594727, -5484.9165039063)
				}
			elseif ((Lv == 1050 or Lv <= 1099) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Winter Warrior") then 
				return {
					[1] = "Winter Warrior [Lv. 1050]";
					[2] = "SnowMountainQuest";
					[3] = 2;
					[4] = "Winter Warrior";
					[5] = CF(607.05963134766, 401.44781494141, -5370.5546875);
					[6] = CF(1234.4449462891, 456.95419311523, -5174.130859375)
				}
			elseif ((Lv == 1100 or Lv <= 1124) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Lab Subordinate") then 
				return {
					[1] = "Lab Subordinate [Lv. 1100]";
					[2] = "IceSideQuest";
					[3] = 1;
					[4] = "Lab Subordinate";
					[5] = CF(-6061.841796875, 15.926671981812, -4902.0385742188);
					[6] = CF(-5720.5576171875, 63.309471130371, -4784.6103515625)
				}
			elseif ((Lv == 1125 or Lv <= 1174) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Horned Warrior") then 
				return {
					[1] = "Horned Warrior [Lv. 1125]";
					[2] = "IceSideQuest";
					[3] = 2;
					[4] = "Horned Warrior";
					[5] = CF(-6061.841796875, 15.926671981812, -4902.0385742188);
					[6] = CF(-6292.751953125, 91.181983947754, -5502.6499023438)
				}
			elseif ((Lv == 1175 or Lv <= 1199) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Magma Ninja") then 
				return {
					[1] = "Magma Ninja [Lv. 1175]";
					[2] = "FireSideQuest";
					[3] = 1;
					[4] = "Magma Ninja";
					[5] = CF(-5429.0473632813, 15.977565765381, -5297.9614257813);
					[6] = CF(-5461.8388671875, 130.36347961426, -5836.4702148438)
				}
			elseif ((Lv == 1200 or Lv <= 1249) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Lava Pirate") then 
				return {
					[1] = "Lava Pirate [Lv. 1200]";
					[2] = "FireSideQuest";
					[3] = 2;
					[4] = "Lava Pirate";
					[5] = CF(-5429.0473632813, 15.977565765381, -5297.9614257813);
					[6] = CF(-5251.1889648438, 55.164535522461, -4774.4096679688)
				}
			elseif ((Lv == 1250 or Lv <= 1274) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Ship Deckhand") then 
				if Configs.Farming.Level.AutoFarm and (Vec3(1040.2927246094, 125.08293151855, 32911.0390625) - selff.Character.HumanoidRootPart.Position).Magnitude > 20000 then
					CommF:InvokeServer("requestEntrance",Vec3(923.21252441406, 126.9760055542, 32852.83203125));
				end;
				return {
					[1] = "Ship Deckhand [Lv. 1250]";
					[2] = "ShipQuest1";
					[3] = 1;
					[4] = "Ship Deckhand";
					[5] = CF(1040.2927246094, 125.08293151855, 32911.0390625);
					[6] = CF(921.12365722656, 125.9839553833, 33088.328125)
				}
			elseif ((Lv == 1275 or Lv <= 1299) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Ship Engineer") then 
				if Configs.Farming.Level.AutoFarm and (Vec3(1040.2927246094, 125.08293151855, 32911.0390625) - selff.Character.HumanoidRootPart.Position).Magnitude > 20000 then
					CommF:InvokeServer("requestEntrance",Vec3(923.21252441406, 126.9760055542, 32852.83203125));
				end;
				return {
					[1] = "Ship Engineer [Lv. 1275]";
					[2] = "ShipQuest1";
					[3] = 2;
					[4] = "Ship Engineer";
					[5] = CF(1040.2927246094, 125.08293151855, 32911.0390625);
					[6] = CF(886.28179931641, 40.47790145874, 32800.83203125)
				}
			elseif ((Lv == 1300 or Lv <= 1324) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Ship Steward") then 
				if Configs.Farming.Level.AutoFarm and (Vec3(1040.2927246094, 125.08293151855, 32911.0390625) - selff.Character.HumanoidRootPart.Position).Magnitude > 20000 then
					CommF:InvokeServer("requestEntrance",Vec3(923.21252441406, 126.9760055542, 32852.83203125));
				end;
				return {
					[1] = "Ship Steward [Lv. 1300]";
					[2] = "ShipQuest2";
					[3] = 1;
					[4] = "Ship Steward";
					[5] = CF(971.42065429688, 125.08293151855, 33245.54296875);
					[6] = CF(943.85504150391, 129.58183288574, 33444.3671875)
				}
			elseif ((Lv == 1325 or Lv <= 1349) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Ship Officer") then 
				if Configs.Farming.Level.AutoFarm and (Vec3(1040.2927246094, 125.08293151855, 32911.0390625) - selff.Character.HumanoidRootPart.Position).Magnitude > 20000 then
					CommF:InvokeServer("requestEntrance",Vec3(923.21252441406, 126.9760055542, 32852.83203125));
				end;
				return {
					[1] = "Ship Officer [Lv. 1325]";
					[2] = "ShipQuest2";
					[3] = 2;
					[4] = "Ship Officer";
					[5] = CF(971.42065429688, 125.08293151855, 33245.54296875);
					[6] = CF(955.38458251953, 181.08335876465, 33331.890625)
				}
			elseif ((Lv == 1350 or Lv <= 1374) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Arctic Warrior") then 
				if Configs.Farming.Level.AutoFarm and (Vec3(5668.1372070313, 28.202531814575, -6484.6005859375) - selff.Character.HumanoidRootPart.Position).Magnitude > 20000 then
					CommF:InvokeServer("requestEntrance",Vec3(-6508.5581054688, 89.034996032715, -132.83953857422));
				end;
				return {
					[1] = "Arctic Warrior [Lv. 1350]";
					[2] = "FrostQuest";
					[3] = 1;
					[4] = "Arctic Warrior";
					[5] = CF(5668.1372070313, 28.202531814575, -6484.6005859375);
					[6] = CF(5935.4541015625, 77.26016998291, -6472.7568359375)
				}
			elseif ((Lv == 1375 or Lv <= 1424) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Snow Lurker") then 
				return {
					[1] = "Snow Lurker [Lv. 1375]";
					[2] = "FrostQuest";
					[3] = 2;
					[4] = "Snow Lurker";
					[5] = CF(5668.1372070313, 28.202531814575, -6484.6005859375);
					[6] = CF(5628.482421875, 57.574996948242, -6618.3481445313)
				}
			elseif ((Lv == 1425 or Lv <= 1449) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Sea Soldier") then 
				return {
					[1] = "Sea Soldier [Lv. 1425]";
					[2] = "ForgottenQuest";
					[3] = 1;
					[4] = "Sea Soldier";
					[5] = CF(-3054.5827636719, 236.87213134766, -10147.790039063);
					[6] = CF(-3185.0153808594, 58.789089202881, -9663.6064453125)
				}
			elseif (Lv >= 1450 and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Water Fighter") then 
				return {
					[1] = "Water Fighter [Lv. 1450]";
					[2] = "ForgottenQuest";
					[3] = 2;
					[4] = "Water Fighter";
					[5] = CF(-3054.5827636719, 236.87213134766, -10147.790039063);
					[6] = CF(-3262.9301757813, 298.69036865234, -10552.529296875)
				}
			end
		elseif WorldCheck["Third Sea"] then
			if ((Lv == 1500 or Lv <= 1524) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Pirate Millionaire") then 
				return {
					[1] = "Pirate Millionaire [Lv. 1500]";
					[2] = "PiratePortQuest";
					[3] = 1;
					[4] = "Pirate Millionaire";
					[5] = CF(-289.61752319336, 43.819011688232, 5580.0903320313);
					[6] = CF(-435.68109130859, 189.69866943359, 5551.0756835938)
				}
			elseif ((Lv == 1525 or Lv <= 1574) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Pistol Billionaire") then 
				return {
					[1] = "Pistol Billionaire [Lv. 1525]";
					[2] = "PiratePortQuest";
					[3] = 2;
					[4] = "Pistol Billionaire";
					[5] = CF(-289.61752319336, 43.819011688232, 5580.0903320313);
					[6] = CF(-236.53652954102, 217.46676635742, 6006.0883789063)
				}
			elseif ((Lv == 1575 or Lv <= 1599) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Dragon Crew Warrior") then 
				return {
					[1] = "Dragon Crew Warrior [Lv. 1575]";
					[2] = "AmazonQuest";
					[3] = 1;
					[4] = "Dragon Crew Warrior";
					[5] = CF(5833.1147460938, 51.60498046875, -1103.0693359375);
					[6] = CF(6301.9975585938, 104.77153015137, -1082.6075439453)
				}
			elseif ((Lv == 1600 or Lv <= 1624) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Dragon Crew Archer") then 
				return {
					[1] = "Dragon Crew Archer [Lv. 1600]";
					[2] = "AmazonQuest";
					[3] = 2;
					[4] = "Dragon Crew Archer";
					[5] = CF(5833.1147460938, 51.60498046875, -1103.0693359375);
					[6] = CF(6831.1171875, 441.76708984375, 446.58615112305)
				}
			elseif ((Lv == 1625 or Lv <= 1649) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Female Islander") then 
				return {
					[1] = "Female Islander [Lv. 1625]";
					[2] = "AmazonQuest2";
					[3] = 1;
					[4] = "Female Islander";
					[5] = CF(5446.8793945313, 601.62945556641, 749.45672607422);
					[6] = CF(5792.5166015625, 848.14392089844, 1084.1818847656)
				}
			elseif ((Lv == 1650 or Lv <= 1699) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Giant Islander") then 
				return {
					[1] = "Giant Islander [Lv. 1650]";
					[2] = "AmazonQuest2";
					[3] = 2;
					[4] = "Giant Islander";
					[5] = CF(5446.8793945313, 601.62945556641, 749.45672607422);
					[6] = CF(5009.5068359375, 664.11071777344, -40.960144042969)
				}
			elseif ((Lv == 1700 or Lv <= 1724) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Marine Commodore") then 
				return {
					[1] = "Marine Commodore [Lv. 1700]";
					[2] = "MarineTreeIsland";
					[3] = 1;
					[4] = "Marine Commodore";
					[5] = CF(2179.98828125, 28.731239318848, -6740.0551757813);
					[6] = CF(2198.0063476563, 128.71075439453, -7109.5043945313)
				}
			elseif ((Lv == 1725 or Lv <= 1774) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Marine Rear Admiral") then 
				return {
					[1] = "Marine Rear Admiral [Lv. 1725]";
					[2] = "MarineTreeIsland";
					[3] = 2;
					[4] = "Marine Rear Admiral";
					[5] = CF(2179.98828125, 28.731239318848, -6740.0551757813);
					[6] = CF(3294.3142089844, 385.41125488281, -7048.6342773438)
				}
			elseif ((Lv == 1775 or Lv <= 1799) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Fishman Raider") then 
				return {
					[1] = "Fishman Raider [Lv. 1775]";
					[2] = "DeepForestIsland3";
					[3] = 1;
					[4] = "Fishman Raider";
					[5] = CF(-10582.759765625, 331.78845214844, -8757.666015625);
					[6] = CF(-10553.268554688, 521.38439941406, -8176.9458007813)
				}
			elseif ((Lv == 1800 or Lv <= 1824) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Fishman Captain") then 
				return {
					[1] = "Fishman Captain [Lv. 1800]";
					[2] = "DeepForestIsland3";
					[3] = 2;
					[4] = "Fishman Captain";
					[5] = CF(-10583.099609375, 331.78845214844, -8759.4638671875);
					[6] = CF(-10789.401367188, 427.18637084961, -9131.4423828125)
				}
			elseif ((Lv == 1825 or Lv <= 1849) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Forest Pirate") then 
				return {
					[1] = "Forest Pirate [Lv. 1825]";
					[2] = "DeepForestIsland";
					[3] = 1;
					[4] = "Forest Pirate";
					[5] = CF(-13232.662109375, 332.40396118164, -7626.4819335938);
					[6] = CF(-13489.397460938, 400.30349731445, -7770.251953125)
				}
			elseif ((Lv == 1850 or Lv <= 1899) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Mythological Pirate") then 
				return {
					[1] = "Mythological Pirate [Lv. 1850]";
					[2] = "DeepForestIsland";
					[3] = 2;
					[4] = "Mythological Pirate";
					[5] = CF(-13232.662109375, 332.40396118164, -7626.4819335938);
					[6] = CF(-13508.616210938, 582.46228027344, -6985.3037109375)
				}
			elseif ((Lv >= 1900 and Lv <= 1924) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Jungle Pirate") then 
				return {
					[1] = "Jungle Pirate [Lv. 1900]";
					[2] = "DeepForestIsland2";
					[3] = 1;
					[4] = "Jungle Pirate";
					[5] = CF(-12682.096679688, 390.88653564453, -9902.1240234375);
					[6] = CF(-12267.103515625, 459.75262451172, -10277.200195313)
				}
			elseif ((Lv >= 1925 and Lv <= 1974) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Musketeer Pirate") then 
				return {
					[1] = "Musketeer Pirate [Lv. 1925]";
					[2] = "DeepForestIsland2";
					[3] = 2;
					[4] = "Musketeer Pirate";
					[5] = CF(-12682.096679688, 390.88653564453, -9902.1240234375);
					[6] = CF(-13291.5078125, 520.47338867188, -9904.638671875)
				}
			elseif ((Lv >= 1975 and Lv <= 1999) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Reborn Skeleton") then 
				return {
					[1] = "Reborn Skeleton [Lv. 1975]";
					[2] = "HauntedQuest1";
					[3] = 1;
					[4] = "Reborn Skeleton";
					[5] = CF(-9479.2168, 141.215088, 5566.09277);
					[6] = CF(-8763.7236328125, 165.72299194335938, 6159.86181640625)
				}
			elseif ((Lv >= 2000 and Lv <= 2024) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Living Zombie") then 
				return {
					[1] = "Living Zombie [Lv. 2000]";
					[2] = "HauntedQuest1";
					[3] = 2;
					[4] = "Living Zombie";
					[5] = CF(-9480.80762, 142.130661, 5566.37305);
					[6] = CF(-10103.7529, 238.565979, 6179.75977)
				}
			elseif ((Lv >= 2025 and Lv <= 2049) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Demonic Soul") then 
				return {
					[1] = "Demonic Soul [Lv. 2025]";
					[2] = "HauntedQuest2";
					[3] = 1;
					[4] = "Demonic Souls";
					[5] = CF(-9516.99316, 172.017181, 6078.46533);
					[6] = CF(-9709.30762, 204.695892, 6044.04688)
				}
			elseif ((Lv >= 2050 and Lv <= 2074) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Posessed Mummy") then 
				return {
					[1] = "Posessed Mummy [Lv. 2050]";
					[2] = "HauntedQuest2";
					[3] = 2;
					[4] = "Posessed Mummys";
					[5] = CF(-9515.39551, 172.266037, 6078.89746);
					[6] = CF(-9554.11035, 65.6141663, 6041.73584)
				}
			elseif ((Lv >= 2075 and Lv <= 2099) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Peanut Scout") then 
				return {
					[1] = "Peanut Scout [Lv. 2075]";
					[2] = "NutsIslandQuest";
					[3] = 1;
					[4] = "Peanut Scout";
					[5] = CF(-2104.453125, 38.129974365234, -10194.0078125);
					[6] = CF(-2068.0949707031, 76.512603759766, -10117.150390625)
				}
			elseif ((Lv >= 2100 and Lv <= 2124) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Peanut President") then 
				return {
					[1] = "Peanut President [Lv. 2100]";
					[2] = "NutsIslandQuest";
					[3] = 2;
					[4] = "Peanut President";
					[5] = CF(-2104.453125, 38.129974365234, -10194.0078125);
					[6] = CF(-2067.33203125, 90.557350158691, -10552.051757812)
				}
			elseif ((Lv >= 2125 and Lv <= 2149) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Ice Cream Chef") then 
				return {
					[1] = "Ice Cream Chef [Lv. 2125]";
					[2] = "IceCreamIslandQuest";
					[3] = 1;
					[4] = "Ice Cream Chef";
					[5] = CF(-821.35913085938, 65.845329284668, -10965.2578125);
					[6] = CF(-796.37261962891, 110.95275878906, -10847.473632812)
				}
			elseif ((Lv >= 2150 and Lv <= 2200) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Ice Cream Commander") then 
				return {
					[1] = "Ice Cream Commander [Lv. 2150]";
					[2] = "IceCreamIslandQuest";
					[3] = 2;
					[4] = "Ice Cream Commander";
					[5] = CF(-821.35913085938, 65.845329284668, -10965.2578125);
					[6] = CF(-697.65338134766, 174.48368835449, -11218.38671875)
				}
			elseif ((Lv >= 2200 and Lv <= 2224) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Cookie Crafter") then 
				return {
					[1] = "Cookie Crafter [Lv. 2200]";
					[2] = "CakeQuest1";
					[3] = 1;
					[4] = "Cookie Crafter";
					[5] = CF(-2017.4874267578125, 36.85276412963867, -12027.53515625);
					[6] = CF(-2358.5791015625, 36.85615539550781, -12111.052734375)
				};
			elseif ((Lv >= 2225 and Lv <= 2249) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Cake Guard") then 
				return {
					[1] = "Cake Guard [Lv. 2225]";
					[2] = "CakeQuest1";
					[3] = 2;
					[4] = "Cake Guard";
					[5] = CF(-2021.32007, 37.7982254, -12028.7295);
					[6] = CF(-1598.3070068359375, 43.773197174072266, -12244.5810546875)
				};
			elseif ((Lv >= 2250 and Lv <= 2274) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Baking Staff") then 
				return {
					[1] = "Baking Staff [Lv. 2250]";
					[2] = "CakeQuest2";
					[3] = 1;
					[4] = "Baking Staff";
					[5] = CF(-1927.91602, 37.7981339, -12842.5391);
					[6] = CF(-1887.8099365234375, 77.6185073852539, -12998.3505859375)
				};
			elseif ((Lv >= 2275 and Lv <= 2299) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Head Baker") then 
				return {
					[1] = "Head Baker [Lv. 2275]";
					[2] = "CakeQuest2";
					[3] = 2;
					[4] = "Head Baker";
					[5] = CF(-1927.91602, 37.7981339, -12842.5391);
					[6] = CF(-2216.188232421875, 82.884521484375, -12869.2939453125)
				};
			elseif ((Lv >= 2300 and Lv <= 2324) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Cocoa Warrior") then 
				return {
					[1] = "Cocoa Warrior [Lv. 2300]";
					[2] = "ChocQuest1";
					[3] = 1;
					[4] = "Cocoa Warrior";
					[5] = CF(233.22836303710938, 29.876001358032227, -12201.2333984375);
					[6] = CF(-21.55328369140625, 80.57499694824219, -12352.3876953125)
				};
			elseif ((Lv >= 2325 and Lv <= 2349) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Chocolate Bar Battler") then 
				return {
					[1] = "Chocolate Bar Battler [Lv. 2325]";
					[2] = "ChocQuest1";
					[3] = 2;
					[4] = "Chocolate Bar Battler";
					[5] = CF(233.22836303710938, 29.876001358032227, -12201.2333984375);
					[6] = CF(582.590576171875, 77.18809509277344, -12463.162109375)
				};
			elseif ((Lv >= 2350 and Lv <= 2374) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Sweet Thief") then 
				return {
					[1] = "Sweet Thief [Lv. 2350]";
					[2] = "ChocQuest2";
					[3] = 1;
					[4] = "Sweet Thief";
					[5] = CF(150.5066375732422, 30.693693161010742, -12774.5029296875);
					[6] = CF(165.1884765625, 76.05885314941406, -12600.8369140625)
				};
			elseif ((Lv >= 2375 and Lv <= 2399) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Candy Rebel") then 
				return {
					[1] = "Candy Rebel [Lv. 2375]";
					[2] = "ChocQuest2";
					[3] = 2;
					[4] = "Candy Rebel";
					[5] = CF(150.5066375732422, 30.693693161010742, -12774.5029296875);
					[6] = CF(134.86563110351562, 77.2476806640625, -12876.5478515625)
				};
			elseif ((Lv >= 2400 and Lv <= 2424) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Candy Pirate") then 
				return {
					[1] = "Candy Pirate [Lv. 2400]";
					[2] = "CandyQuest1";
					[3] = 1;
					[4] = "Candy Pirate";
					[5] = CF(-1150.0400390625, 20.378934860229492, -14446.3349609375);
					[6] = CF(-1310.5003662109375, 26.016523361206055, -14562.404296875)
				};
			elseif ((Lv >= 2425 and Lv <= 2449) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Snow Demon") then 
				return {
					[1] = "Snow Demon [Lv. 2449]";
					[2] = "CandyQuest1";
					[3] = 2;
					[4] = "Snow Demon";
					[5] = CF(-1150.0400390625, 20.378934860229492, -14446.3349609375);
					[6] = CF(-880.2006225585938, 71.24776458740234, -14538.609375)
				};
			elseif ((Lv >= 2450 and Lv <= 2474) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Isle Outlaw") then 
				return {
					[1] = "Isle Outlaw [Lv. 2450]";
					[2] = "TikiQuest1";
					[3] = 1;
					[4] = "Isle Outlaw";
					[5] = CF(-16547.748046875, 61.13533401489258, -173.41360473632812);
					[6] = CF(-16442.814453125, 116.13899993896484, -264.4637756347656)
				};
			elseif ((Lv >= 2475 and Lv <= 2524) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Island Boy") then 
				return {
					[1] = "Island Boy [Lv. 2475]";
					[2] = "TikiQuest1";
					[3] = 2;
					[4] = "Island Boy";
					[5] = CF(-16547.748046875, 61.13533401489258, -173.41360473632812);
					[6] = CF(-16901.26171875, 84.06756591796875, -192.88906860351562)
				};
			elseif ((Lv >= 2525 and Lv <= 2550) and not Configs.System.Selected_Mob.Auto) or (Configs.System.Selected_Mob.Auto and Configs.System.Selected_Mob.Obj == "Isle Champion") then 
				return {
					[1] = "Isle Champion [Lv. 2525]";
					[2] = "TikiQuest2";
					[3] = 2;
					[4] = "Isle Champion";
					[5] = CF(-16539.078125, 55.68632888793945, 1051.5738525390625);
					[6] = CF(-16641.6796875, 235.7825469970703, 1031.282958984375)
				};
			end;
		end;
	end;

	function FindBoat()
		for _, v in next, GetChildren(W.Boats) do
			if FindFirstChild(v, "Owner") and dist(GetPivot(v).Position) <= 300 then
				if v.Owner.Value == game.Players.LocalPlayer then
					return v;
				end;
			end;
		end; twait();
		return nil;
	end;

	function SetKillTarget(name, configValue)
		repeat twait();
			for i,v in pir(GetChildren(Enemy)) do
				if v.Name == name then
					if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
						sethiddenproperty(selff, "SimulationRadius", math.huge);
						TargetMobName = v.Name;
						repeat twait();
							Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed or 4);
							SetHum(v); Hitc();
						until not v or not configValue or v.Humanoid.Health <= 0
					end;
				end;
			end;
		until not configValue or not FindFirstChild(Enemy, name);
	end;

	local ResetRaid = false;
	function AutoRaid(obj)
		local FruitToCompare = {}
		for i, v in pir(CommF:InvokeServer("getInventory")) do
			if type(v) == "table" then
				if rawget(v, "Value") and v.Value < 600000 then
					tablein(FruitToCompare, v.Value);
				end;
			end;
		end; wait(0.3);
		local MaxPrice = 600000
		local smallestPrice = MaxPrice
		for i, v in pairs(FruitToCompare) do
			if v < smallestPrice then
				smallestPrice = v;
			end;
		end; twait();
		local SmalFruit = nil;
		for i, v in pir(CommF:InvokeServer("getInventory")) do
			if type(v) == "table" then
				if rawget(v, "Value") and v.Value == smallestPrice then
					SmalFruit = v.Name;
				end;
			end;
		end; twait(0.3);
		IgnoreE = true;
		repeat twait()
			if FindFirstChildWhichIsA(selc, "Tool") then
				FindFirstChildWhichIsA(selc, "Tool").Parent = selff.Backpack
			end;
		until not FindFirstChildWhichIsA(selc, "Tool");
		twait(0.3);
		if SmalFruit ~= nil then
			CommF:InvokeServer("LoadFruit", SmalFruit); twait(0.3);
			CommF:InvokeServer("RaidsNpc", "Select", "Dark");
			if not PSG.Main.Timer.Visible and (not FindFirstChild(W._WorldOrigin.Locations, "Island 1") or (FindFirstChild(W._WorldOrigin.Locations, "Island 1") and dist(FindFirstChild(W._WorldOrigin.Locations, "Island 1").CFrame.Position) > 6700)) then
				if WorldCheck["Second Sea"] then
					fireclickdetector(W.Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector);
				elseif WorldCheck["Third Sea"] then
					fireclickdetector(W.Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector);
				end;
				repeat twait(); until (FindFirstChild(W._WorldOrigin.Locations, "Island 1") and dist(FindFirstChild(W._WorldOrigin.Locations, "Island 1").CFrame.Position) <= 6700) or not selc or (selc and not FindFirstChild(selc, "Humanoid")) or selc.Humanoid.Health <= 0;
				tspawn(function()
					repeat twait();
						if (not FindFirstChild(LocationsFol, "Island 5") and not FindFirstChild(LocationsFol, "Island 4") and not FindFirstChild(LocationsFol, "Island 3") and not FindFirstChild(LocationsFol, "Island 2") and not FindFirstChild(LocationsFol, "Island 1")) or (FindFirstChild(LocationsFol, "Island 1") and dist(FindFirstChild(LocationsFol, "Island 1").CFrame.Position) > 6700) then
							Configs.System.BREAKALLTHISSHITHAHAHAHAHA = true; wait(3); Configs.System.BREAKALLTHISSHITHAHAHAHAHA = false;
						end;
					until ResetRaid
				end);
				LocationsFol = W._WorldOrigin.Locations
				if FindFirstChild(LocationsFol, "Island 1") and dist(FindFirstChild(LocationsFol, "Island 1").CFrame.Position) < 4000 then
					repeat twait();
						if FindFirstChild(LocationsFol, "Island 5") then
							Tp(FindFirstChild(LocationsFol, "Island 5").CFrame*CF(0 + 0, 25 + 0, 0 + 0), 2.5);
						elseif FindFirstChild(LocationsFol, "Island 4") then
							Tp(FindFirstChild(LocationsFol, "Island 4").CFrame*CF(0 + 0, 25 + 0, 0 + 0), 2.5);
						elseif FindFirstChild(LocationsFol, "Island 3") then
							Tp(FindFirstChild(LocationsFol, "Island 3").CFrame*CF(0 + 0, 25 + 0, 0 + 0), 2.5);
						elseif FindFirstChild(LocationsFol, "Island 2") then
							Tp(FindFirstChild(LocationsFol, "Island 2").CFrame*CF(0 + 0, 25 + 0, 0 + 0), 2.5);
						elseif FindFirstChild(LocationsFol, "Island 1") then
							Tp(FindFirstChild(LocationsFol, "Island 1").CFrame*CF(0 + 0, 25 + 0, 0 + 0), 2.5);
						end;
					until (not FindFirstChild(LocationsFol, "Island 5") and not FindFirstChild(LocationsFol, "Island 4") and not FindFirstChild(LocationsFol, "Island 3") and not FindFirstChild(LocationsFol, "Island 2") and not FindFirstChild(LocationsFol, "Island 1")) or (FindFirstChild(LocationsFol, "Island 1") and dist(FindFirstChild(LocationsFol, "Island 1").CFrame.Position) > 6700)
				end;
			else
				repeat twait(); until (FindFirstChild(W._WorldOrigin.Locations, "Island 1") and dist(FindFirstChild(W._WorldOrigin.Locations, "Island 1").CFrame.Position) <= 6700) or not selc or (selc and not FindFirstChild(selc, "Humanoid")) or selc.Humanoid.Health <= 0;
				warn("a")
				tspawn(function()
					repeat twait();
						if (not FindFirstChild(LocationsFol, "Island 5") and not FindFirstChild(LocationsFol, "Island 4") and not FindFirstChild(LocationsFol, "Island 3") and not FindFirstChild(LocationsFol, "Island 2") and not FindFirstChild(LocationsFol, "Island 1")) or (FindFirstChild(LocationsFol, "Island 1") and dist(FindFirstChild(LocationsFol, "Island 1").CFrame.Position) > 6700) then
							Configs.System.BREAKALLTHISSHITHAHAHAHAHA = true; wait(3); Configs.System.BREAKALLTHISSHITHAHAHAHAHA = false;
						end;
					until ResetRaid
				end);
				warn("b")
				LocationsFol = W._WorldOrigin.Locations
				if FindFirstChild(LocationsFol, "Island 1") and dist(FindFirstChild(LocationsFol, "Island 1").CFrame.Position) < 4000 then
					repeat twait();
						if FindFirstChild(LocationsFol, "Island 5") then
							Tp(FindFirstChild(LocationsFol, "Island 5").CFrame*CF(0 + 0, 25 + 0, 0 + 0), 2.5);
						elseif FindFirstChild(LocationsFol, "Island 4") then
							Tp(FindFirstChild(LocationsFol, "Island 4").CFrame*CF(0 + 0, 25 + 0, 0 + 0), 2.5);
						elseif FindFirstChild(LocationsFol, "Island 3") then
							Tp(FindFirstChild(LocationsFol, "Island 3").CFrame*CF(0 + 0, 25 + 0, 0 + 0), 2.5);
						elseif FindFirstChild(LocationsFol, "Island 2") then
							Tp(FindFirstChild(LocationsFol, "Island 2").CFrame*CF(0 + 0, 25 + 0, 0 + 0), 2.5);
						elseif FindFirstChild(LocationsFol, "Island 1") then
							Tp(FindFirstChild(LocationsFol, "Island 1").CFrame*CF(0 + 0, 25 + 0, 0 + 0), 2.5);
						end;
					until (not FindFirstChild(LocationsFol, "Island 5") and not FindFirstChild(LocationsFol, "Island 4") and not FindFirstChild(LocationsFol, "Island 3") and not FindFirstChild(LocationsFol, "Island 2") and not FindFirstChild(LocationsFol, "Island 1")) or (FindFirstChild(LocationsFol, "Island 1") and dist(FindFirstChild(LocationsFol, "Island 1").CFrame.Position) > 6700)
				end;
			end;
			Notification.new({
				Description = "Reseting.. wait 10.87 seconds and script will continue to auto raid";
				Title = "Neuron X [Raid]";
				Duration = 3;
			});
			twait(3);
			ResetRaid = true;
			IgnoreE = false;
			twait(7.87);
			ResetRaid = false;
			IgnoreE = false;
			Notification.new({
				Description = "Finish";
				Title = "Neuron X [Raid]";
				Duration = 3;
			});
		end; twait();
		IgnoreE = false;
	end;

	local Windows = NothingLibrary.new({
		Title = "Blox Fruit [Full Acess Settings]",
		Description = "By Neuron X [Optimize Version 1.0.1]",
		Keybind = Enum.KeyCode.T,
		Logo = 'http://www.roblox.com/asset/?id=80055499055359'
	});
	local TabFrame = Windows:NewTab({
		Title = "General",
		Description = "Main section",
		Icon = "rbxassetid://7743870134"
	});
	local Section = TabFrame:NewSection({
		Title = "Weapon",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	SWW = Section:NewDropdown({
		Title = "Select Weapon",
		Data = {"nil"},
		Default = "nil",
		Callback = function(a)
			Configs.Farming.Weapon = a.Name;
		end,
	});
	Section:NewButton({
		Title = "Refresh Weapons",
		Callback = function()
			SWW.Set(BP());
		end,
	});
	local Section = TabFrame:NewSection({
		Title = "Stats",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	Section:NewToggle({
		Title = "Melee",
		Default = Configs.Stats.Melee,
		Callback = function(tr)
			Configs.Stats.Melee = tr;
		end,
	});
	Section:NewToggle({
		Title = "Defense",
		Default = Configs.Stats.Defense,
		Callback = function(tr)
			Configs.Stats.Defense = tr;
		end,
	});
	Section:NewToggle({
		Title = "Sword",
		Default = Configs.Stats.Sword,
		Callback = function(tr)
			Configs.Stats.Sword = tr;
		end,
	});
	Section:NewToggle({
		Title = "Gun",
		Default = Configs.Stats.Gun,
		Callback = function(tr)
			Configs.Stats.Gun = tr;
		end,
	});
	Section:NewToggle({
		Title = "BloxFruit",
		Default = Configs.Stats.BloxFruit,
		Callback = function(tr)
			Configs.Stats.BloxFruit = tr;
		end,
	});
	Section:NewSlider({
		Title = "Points",
		Min = 1,
		Max = 50,
		Default = Configs.Stats.Points,
		Callback = function(a)
			Configs.Stats.Points = a;
		end,
	})
	local Section = TabFrame:NewSection({
		Title = "Farming | Level",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	Section:NewToggle({
		Title = "Auto Farm",
		Default = Configs.Farming.Level.AutoFarm,
		Callback = function(tr)
			Configs.Farming.Level.AutoFarm = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Sea 2",
		Default = Configs.Farming.Level.AutoSea2,
		Callback = function(tr)
			Configs.Farming.Level.AutoSea2 = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Sea 3",
		Default = Configs.Farming.Level.AutoSea3,
		Callback = function(tr)
			Configs.Farming.Level.AutoSea3 = tr;
		end,
	});

	local Section = TabFrame:NewSection({
		Title = "Farming | Boss",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	RFFBOS = Section:NewDropdown({
		Title = "Select Boss",
		Data = {"nil"},
		Default = "nil",
		Callback = function(a)
			Configs.System.Selected_Boss.Obj = a;
		end,
	});
	Section:NewButton({
		Title = "Refresh Boss",
		Callback = function()
			NewValueTable = {};
			for i=1, #BossList do
				if FindFirstChild(R, BossList[i]) then
					table.insert(NewValueTable, R[BossList[i]].Name);
				end;
			end; twait();
			for i=1, #BossList do
				if FindFirstChild(Enemy, BossList[i]) then
					table.insert(NewValueTable, Enemy[BossList[i]].Name);
				end;
			end; twait();
			RFFBOS.Set(NewValueTable);
		end,
	});
	Section:NewToggle({
		Title = "Auto Farm Selected Boss",
		Default = false,
		Callback = function(tr)
			Configs.System.Selected_Boss.Auto = tr;
		end,
	});



	local Section = TabFrame:NewSection({
		Title = "Farming | Selected Mob",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	Section:NewDropdown({
		Title = "Select Mob",
		Data = MobList,
		Default = MobList[1],
		Callback = function(a)
			Configs.System.Selected_Mob.Obj = a;
		end,
	});
	Section:NewToggle({
		Title = "Auto Farm Selected Mob",
		Default = false,
		Callback = function(tr)
			Configs.System.Selected_Mob.Auto = tr;
		end,
	});

	local Section = TabFrame:NewSection({
		Title = "Farming | Chest",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	Section:NewToggle({
		Title = "Auto Chest",
		Default = Configs.System.AutoChest,
		Callback = function(tr)
			Configs.System.AutoChest = tr;
		end,
	});

	local Section = TabFrame:NewSection({
		Title = "Farming | Materials",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	Section:NewDropdown({
		Title = "Select Material",
		Data = {"nil"},
		Default = "nil",
		Callback = function(a)
			Configs.System.Selected_Material.Obj = a;
		end,
	});
	Section:NewToggle({
		Title = "Auto Farm Selected Material",
		Default = false,
		Callback = function(tr)
			Configs.System.Selected_Material.Auto = tr;
		end,
	});

	local Section = TabFrame:NewSection({
		Title = "Farming | All Sea",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	Section:NewToggle({
		Title = "Auto Hop (When need, Sea)",
		Default = false,
		Callback = function(tr)
			
		end,
	});
	Section:NewToggle({
		Title = "Auto Hop (When need, Server)",
		Default = false,
		Callback = function(tr)
			
		end,
	});
	Section:NewToggle({
		Title = "Auto Dragon Breath",
		Default = false,
		Callback = function(tr)
			
		end,
	});
	Section:NewToggle({
		Title = "Auto Death Step",
		Default = Configs.Farming.AllSea.Melee.DeathStep,
		Callback = function(tr)
			Configs.Farming.AllSea.Melee.DeathStep = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Sharkman Karate",
		Default = Configs.Farming.AllSea.Melee.SharkmanKarate,
		Callback = function(tr)
			Configs.Farming.AllSea.Melee.SharkmanKarate = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Superhuman",
		Default = false,
		Callback = function(tr)
			
		end,
	});
	Section:NewToggle({
		Title = "Auto Electric Claw",
		Default = false,
		Callback = function(tr)
			
		end,
	});
	Section:NewToggle({
		Title = "Auto Dragon Talon",
		Default = false,
		Callback = function(tr)
			
		end,
	});
	Section:NewToggle({
		Title = "Auto Godhuman",
		Default = false,
		Callback = function(tr)
			
		end,
	});
	Section:NewToggle({
		Title = "Auto Sanguine Art (Unstackable)",
		Default = false,
		Callback = function(tr)
			
		end,
	});

	local Section = TabFrame:NewSection({
		Title = "Farming | Sea 1",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	Section:NewToggle({
		Title = "Auto Saber Quest",
		Default = false,
		Callback = function(tr)
			Configs.Farming.Sea1.AutoSaberQuest = tr;
		end,
	});

	Section:NewToggle({
		Title = "Auto Saber",
		Default = false,
		Callback = function(tr)
			Configs.Farming.Sea1.AutoSaber = tr;
		end,
	});
	Section:NewToggle({
		Title = "Hop (Saber)",
		Default = false,
		Callback = function(tr)
			Configs.Farming.Sea1.Hop = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Saw",
		Default = false,
		Callback = function(tr)
			Configs.Farming.Sea1.AutoSaw = tr;
		end,
	});
	Section:NewToggle({
		Title = "Hop (Saw)",
		Default = false,
		Callback = function(tr)
			Configs.Farming.Sea1.HopSaw = tr;
		end,
	});

	local Section = TabFrame:NewSection({
		Title = "Farming | Sea 2",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	Section:NewToggle({
		Title = "Auto Factory",
		Default = false,
		Callback = function(tr)
			Configs.Farming.Sea2.AutoFactory = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Ectoplasm",
		Default = false,
		Callback = function(tr)
			Configs.Farming.Sea2.AutoEctoplasm = tr;
		end,
	});
	ABTLITOAQUET = Section:NewToggle({
		Title = "Auto Bartilo",
		Default = false,
		Callback = function(tr)
			Configs.Farming.Sea2.AutoBartilo = tr;
		end,
	});


	local Section = TabFrame:NewSection({
		Title = "Farming | Sea 3",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	Section:NewToggle({
		Title = "Auto Kill Dough King",
		Default = Configs.Farming.Sea3.AutoKillDoughKing ,
		Callback = function(tr)
			Configs.Farming.Sea3.AutoKillDoughKing = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Castle Raid",
		Default = Configs.Farming.Sea3.AutoCastleRaid ,
		Callback = function(tr)
			if tr then
				Notification.new({
					Description = "Functions not found. The script does not support <Auto Castle Raid>";
					Title = "Neuron X [Error]";
					Duration = 3;
				});
			end;
		end,
	});
	EliteHunterSpawnT = RepText(Section, 20, "Elite Spawn: " .. tos(false));
	EliteKilledT = RepText(Section, 20, "Elite Killed: " .. 0);
	Section:NewToggle({
		Title = "Auto Elite Hunter",
		Default = Configs.Farming.Sea3.AutoElitehunter,
		Callback = function(tr)
			Configs.Farming.Sea3.AutoElitehunter = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Farm Bone",
		Default = Configs.Farming.Sea3.AutoFarmBone,
		Callback = function(tr)
			Configs.Farming.Sea3.AutoFarmBone = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Spawn Cake Prince",
		Default = Configs.Farming.Sea3.AutoSpawnCakePrince,
		Callback = function(tr)
			Configs.Farming.Sea3.AutoSpawnCakePrince = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Kill Cake Prince",
		Default = Configs.Farming.Sea3.AutoCakePrince,
		Callback = function(tr)
			Configs.Farming.Sea3.AutoCakePrince = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Cake Prince(Seperated)",
		Default = Configs.Farming.Sea3.AutoFarmCakePrince,
		Callback = function(tr)
			Configs.Farming.Sea3.AutoFarmCakePrince = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Buddy Sword",
		Default = Configs.Farming.Sea3.AutoBuddySword,
		Callback = function(tr)
			Configs.Farming.Sea3.AutoBuddySword = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Carvender",
		Default = Configs.Farming.Sea3.AutoCarvender ,
		Callback = function(tr)
			Configs.Farming.Sea3.AutoCarvender = tr;
		end,
	});
	Section:NewToggle({
		Title = "Hop (Carvender)",
		Default = Configs.Farming.Sea3.AutoCarvenderHop ,
		Callback = function(tr)
			Configs.Farming.Sea3.AutoCarvenderHop = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto ArenaTrainer",
		Default = Configs.Farming.Sea3.ArenaTrainer ,
		Callback = function(tr)
			Configs.Farming.Sea3.ArenaTrainer = tr;
			if tr then
				Notification.new({
					Description = "Functions not found. The script does not support <Auto ArenaTrainer>";
					Title = "Neuron X [Error]";
					Duration = 3;
				});
			end;
		end,
	});
	Section:NewToggle({
		Title = "Full Auto Azure Ember",
		Default = Configs.Farming.Sea3.FullAutoAzureEmber ,
		Callback = function(tr)
			Configs.Farming.Sea3.FullAutoAzureEmber = tr;
			if tr then
				Notification.new({
					Description = "This will only work if someone in server found kitsune island";
					Title = "Neuron X [Guide]";
					Duration = 5;
				});
				Notification.new({
					Description = "If you wish to use auto find kitsune island with auto enmber, Go to Sea event tab";
					Title = "Neuron X [Guide]";
					Duration = 9;
				});
			end;
		end,
	});



	local TabFrame = Windows:NewTab({
		Title = "Players",
		Description = "",
		Icon = "rbxassetid://7743870134"
	});
	local Section = TabFrame:NewSection({
		Title = "Client",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	Section:NewToggle({
		Title = "Anti AFK",
		Default = Configs.Players.Client.AntiAFK,
		Callback = function(tr)
			Configs.Players.Client.AntiAFK = tr;
		end,
	});
	Section:NewToggle({
		Title = "Float",
		Default = Configs.Players.Client.Float,
		Callback = function(tr)
			Configs.Players.Client.Float = tr;
		end,
	});
	Section:NewToggle({
		Title = "Noclip",
		Default = Configs.Players.Client.Noclip,
		Callback = function(tr)
			Configs.Players.Client.Noclip = tr;
		end,
	});
	Section:NewSlider({
		Title = "Fly Speed",
		Min = 1,
		Max = 10,
		Default = Configs.Players.Client.FlySpeed,
		Callback = function(a)
			Configs.Players.Client.FlySpeed = a;
		end,
	});
	Section:NewToggle({
		Title = "Fly",
		Default = false,
		Callback = function(tr)
			Configs.Players.Client.Fly = tr;
		end,
	});
	Section:NewToggle({
		Title = "DMG Aura",
		Default = Configs.Players.Client.DMGAura,
		Callback = function(tr)
			Configs.Players.Client.DMGAura = tr;
		end,
	});
	Section:NewToggle({
		Title = "Infinite Stamina",
		Default = Configs.Players.Client.InfiniteEnergy,
		Callback = function(tr)
			Configs.Players.Client.InfiniteEnergy = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Buso Haki",
		Default = Configs.Players.Client.AutoBusoHaki,
		Callback = function(tr)
			Configs.Players.Client.AutoBusoHaki = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto OBV Haki",
		Default = Configs.Players.Client.AutoOBVHaki,
		Callback = function(tr)
			Configs.Players.Client.AutoOBVHaki = tr;
		end,
	});
	Section:NewToggle({
		Title = "Walk on water",
		Default = Configs.Players.Client.WalkOnWater,
		Callback = function(tr)
			Configs.Players.Client.WalkOnWater = tr;
		end,
	});
	Section:NewSlider({
		Title = "WalkSpeed (Mutiplier)",
		Min = 1,
		Max = 12,
		Default = Configs.Players.Client.WalkSpeed,
		Callback = function(a)
			Configs.Players.Client.WalkSpeed = a;
		end,
	});
	Section:NewToggle({
		Title = "Toggle WalkSpeed",
		Default = Configs.Players.Client.WalkSpeedToggle,
		Callback = function(tr)
			Configs.Players.Client.WalkSpeedToggle = tr;
		end,
	});
	Section:NewSlider({
		Title = "JumpPower",
		Min = 1,
		Max = 360,
		Default = Configs.Players.Client.JumpPower,
		Callback = function(a)
			Configs.Players.Client.JumpPower = a;
		end,
	});
	Section:NewToggle({
		Title = "Toggle JumpPower",
		Default = Configs.Players.Client.JumpPowerToggle,
		Callback = function(tr)
			Configs.Players.Client.JumpPowerToggle = tr;
		end,
	});
	Section:NewSlider({
		Title = "Max FPS Cap",
		Min = 7,
		Max = 240,
		Default = Configs.Players.Client.MaxFPSCap,
		Callback = function(a)
			setfpscap(a)
		end,
	});
	Section:NewSlider({
		Title = "Dash Speed",
		Min = 1,
		Max = 100,
		Default = (selff and selff.Character and selff.Character:GetAttribute("DashSpeed")) or 1,
		Callback = function(a)
			selff.Character:SetAttribute("DashSpeed", a);
		end,
	});
	Section:NewSlider({
		Title = "DashLength",
		Min = 1,
		Max = 360,
		Default = (selff and selff.Character and selff.Character:GetAttribute("DashLength")) or 1,
		Callback = function(a)
			selff.Character:SetAttribute("DashLength", a);
		end,
	});
	Section:NewToggle({
		Title = "No Render",
		Default = Configs.Players.Client.NoRender,
		Callback = function(tr)
			Configs.Players.Client.NoRender = tr;
			if tr then
				H:Set3dRenderingEnabled(false);
			else
				H:Set3dRenderingEnabled(true);
			end;
		end,
	});
	Section:NewToggle({
		Title = "Black Screen",
		Default = Configs.Players.Client.BlackScreen,
		Callback = function(tr)
			Configs.Players.Client.BlackScreen = tr;
			if tr then
				PSG.Main.Blackscreen.Size = UDim2.new(500, 0, 500, 500);
			else
				PSG.Main.Blackscreen.Size = UDim2.new(1, 0, 500, 500);
			end;
		end,
	});


	local Section = TabFrame:NewSection({
		Title = "Combat",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	local TargetPlayerD = Section:NewDropdown({
		Title = "Select Target Player",
		Data = {"GAY"},
		Default = 1,
		Callback = function(a)
			Configs.Players.Combat.Target = a;
		end,
	});
	Section:NewButton({
		Title = "Refresh Players",
		Callback = function()
			TargetPlayerD.Clear();
			AllPlayers = {};
			AllP = P:GetChildren();
			for i=1, #AllP do
				tablein(AllPlayers, AllP[i]);
			end; twait();
			TargetPlayerD.Set(AllPlayers);
		end,
	});
	Section:NewButton({
		Title = "Get Information Target",
		Callback = function()
			if Lib then
				InformationTargetTargetting = Configs.Players.Combat.Target;
				InformationTargetRequestData = {
					["Name"] = InformationTargetTargetting.Name;
					["Backpack"] = {};
					["Bounty/Honor"] = InformationTargetTargetting.leaderstats["Bounty/Honor"].Value;
					["Data"] = {};
					["IsRaiding"] = InformationTargetTargetting:GetAttribute("IslandRaiding");
					["Yoru Evo"] = InformationTargetTargetting:GetAttribute("YoruEvolution");
					["Bisento Evo"] = InformationTargetTargetting:GetAttribute("BisentoEvolution");
					["Distance"] = dist(InformationTargetTargetting.Character:GetPivot().Position);
					["Player Id"] = InformationTargetTargetting.UserId;
					["Team"] = InformationTargetTargetting.Team;
					["Health"] = InformationTargetTargetting.Character.Humanoid.Health;
					["In combat"] = InformationTargetTargetting.Character:GetAttribute("InCombat");
					["Energy"] = InformationTargetTargetting.Character.Energy.Value;
					["Busy"] = InformationTargetTargetting.Character.Busy.Value;
					["Stun"] = InformationTargetTargetting.Character.Stun.Value;
					["Tool Equiped"] = InformationTargetTargetting.Character:FindFirstChildWhichIsA("Tool");
					["Tool Equiped Level"] = InformationTargetTargetting.Character:FindFirstChildWhichIsA("Tool") and InformationTargetTargetting.Character:FindFirstChildWhichIsA("Tool"):GetAttribute("Level") or "Level not found"
				};
				for _, v in pairs(InformationTargetTargetting.Data:GetChildren()) do
					if v.Name ~= "Stats" and v.Name ~= "Stars" then
						InformationTargetRequestData.Data[v.Name] = v.Value;
					end;
				end;
				InformationTargetRequestData = ("Info = " .. tableTostring(InformationTargetRequestData, 1) .. ";");
				Lib:SendRequest(InformationTargetRequestData);
			else
				Notification.new({
					Description = "Please load extra asset first.";
					Title = "Neuron X [Failed]";
					Duration = 3;
				});
			end;
		end,
	});
	Section:NewButton({
		Title = "Teleport To Target",
		Callback = function()
			--AnyWhereBypass(Configs.Players.Combat.Target.Character:GetPivot());
			Tp(GetPivot(Configs.Players.Combat.Target.Character), Configs.Teleport.TP_Speed);
		end,
	});
	Section:NewToggle({
		Title = "Spectate Target",
		Default = false,
		Callback = function(tr)
			Configs.Players.Combat.Spectate = tr;
			if tr then
				Cam.CameraSubject = Configs.Players.Combat.Target.Character.Humanoid;
			else
				Cam.CameraSubject = selc.Humanoid
			end;
		end,
	});
	Section:NewToggle({
		Title = "Auto Kill Target",
		Default = false,
		Callback = function(tr)
			Configs.Players.Combat.AutoKillTarget = tr;
		end,
	});
	Section:NewToggle({
		Title = "AimBot",
		Default = false,
		Callback = function(tr)
			Configs.Players.Combat.Aimbot = tr;
		end,
	});
	Section:NewToggle({
		Title = "Slient Aim",
		Default = false,
		Callback = function(tr)
			Configs.Players.Combat.SilentAim = tr;
		end,
	});
	Section:NewToggle({
		Title = "Auto Bounty",
		Default = false,
		Callback = function(tr)
			Configs.Players.Combat.AutoBounty = tr;
		end,
	});
	local Section = TabFrame:NewSection({
		Title = "Visual",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	Section:NewToggle({
		Title = "Hide Damage",
		Default = Configs.Players.Client.HideDamageText,
		Callback = function(tr)
			Configs.Players.Client.HideDamageText = tr;
			if tr then
				R.Assets.GUI.DamageCounter.Enabled = false;
			else
				R.Assets.GUI.DamageCounter.Enabled = true;
			end;
		end,
	});
	Section:NewToggle({
		Title = "Hide Notifications",
		Default = Configs.Players.Client.HideDamageText,
		Callback = function(tr)
			Configs.Players.Client.HideDamageText = tr;
			if tr then
				PSG.Notifications.Enabled = false;
			else
				PSG.Notifications.Enabled = true;
			end;
		end,
	});
	Section:NewButton({
		Title = "Fruit Rain",
		Callback = function()
			for h, i in pir((game:GetObjects("rbxassetid://14759368201"))[1]:GetChildren()) do
				i.Parent = game.Workspace.Map;
				i:MoveTo(selff.Character.PrimaryPart.Position + Vector3.new(math.random((-50), 50), 100, math.random((-50), 50)));
				if FindFirstChild(i.Fruit, "AnimationController") then
					((FindFirstChild(i.Fruit, "AnimationController")):LoadAnimation(FindFirstChild(i.Fruit, "Idle"))):Play();
				end;
				i.Handle.Touched:Connect(function(cR)
					if cR.Parent == selff.Character then
						i.Parent = selff.Backpack;
						selff.Character.Humanoid:EquipTool(i);
					end;
				end); twait(0.3)
			end;
		end,
	});
	Section:NewButton({
		Title = "Open Devil Shop",
		Callback = function()
			CommF:InvokeServer("GetFruits");
			PSG.Main.FruitShop.Visible = true;
		end,
	});
	Section:NewButton({
		Title = "Open Haki Color",
		Callback = function()
			CommF:InvokeServer("GetFruits");
			PSG.Main.Colors.Visible = true;
		end,
	});
	Section:NewButton({
		Title = "Open Title",
		Callback = function()
			CommF:InvokeServer("getTitles");
			PSG.Main.Titles.Visible = true;
		end,
	});
	Section:NewToggle({
		Title = "Hide Chat",
		Default = Configs.Players.Client.HideChat,
		Callback = function(tr)
			Configs.Players.Client.HideChat = tr;
			if tr then
				game:GetService("StarterGui"):SetCoreGuiEnabled(Enum.CoreGuiType.Chat, false);
			else
				game:GetService("StarterGui"):SetCoreGuiEnabled(Enum.CoreGuiType.Chat, true);
			end;
		end,
	});
	Section:NewToggle({
		Title = "Hide Leaderboard",
		Default = Configs.Players.Client.HideLeaderboard,
		Callback = function(tr)
			Configs.Players.Client.HideLeaderboard = tr;
			if tr then
				game:GetService("StarterGui"):SetCoreGuiEnabled(Enum.CoreGuiType.PlayerList, false);
			else
				game:GetService("StarterGui"):SetCoreGuiEnabled(Enum.CoreGuiType.PlayerList, true);
			end;
		end,
	});


	TeamSection = TabFrame:NewSection({
		Title = "Team",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	TeamSection:NewButton({
		Title = "Pirates Team",
		Callback = function()
			CommF:InvokeServer("SetTeam", "Pirates");
		end,
	});
	TeamSection:NewButton({
		Title = "Marines Team",
		Callback = function()
			CommF:InvokeServer("SetTeam", "Marines");
		end,
	});





	local TabFrame = Windows:NewTab({
		Title = "Raid",
		Description = "",
		Icon = "rbxassetid://7743870134"
	});
	local Section = TabFrame:NewSection({
		Title = "Manual",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	SRM = Section:NewDropdown({
		Title = "Select Raid",
		Data = {"Flame","Ice","Quake","Light","Dark","String","Rumble","Magma","Human: Buddha","Sand","Bird: Phoenix"},
		Default = Configs.Raid.Manual.SelectRaid,
		Callback = function(a)
			Configs.Raid.Manual.SelectRaid = tr;
		end,
	});
	Section:NewButton({
		Title = "Buy",
		Callback = function()
			CommF:InvokeServer("RaidsNpc", "Select", Configs.Raid.Manual.SelectRaid);
		end,
	});
	Section:NewButton({
		Title = "Start",
		Callback = function()
			if WorldCheck["Second Sea"] then
				fireclickdetector(W.Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector, 0);
			elseif WorldCheck["Third Sea"] then
				fireclickdetector(W.Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector, 0);
			end;
		end,
	});
	Section:NewToggle({
		Title = "Teleport To Next Island",
		Default = false,
		Callback = function(tr)
			
		end,
	});
	Section:NewToggle({
		Title = "Kill Aura",
		Default = Configs.Raid.Manual.KillAura,
		Callback = function(tr)
			Configs.Raid.Manual.KillAura = tr;
		end,
	});
	local Section = TabFrame:NewSection({
		Title = "Auto",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	SRA = Section:NewDropdown({
		Title = "Select Raid",
		Data = {"nil"},
		Default = "nil",
		Callback = function(a)
			print(a);
		end,
	});
	Section:NewToggle({
		Title = "Auto Raid",
		Default = false,
		Callback = function(tr)
			Configs.System.AutoRaid = tr;
		end,
	});



	ShopFrame = Windows:NewTab({
		Title = "Shop",
		Description = "",
		Icon = "rbxassetid://7743870134"
	});
	LegendarySection = ShopFrame:NewSection({
		Title = "Legendary Sword",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	LegendarySection:NewToggle({
		Title = "Auto buy",
		Default = Configs.Shop.AutoBuyLegendarySword,
		Callback = function(tr)
			Configs.Shop.AutoBuyLegendarySword = tr;
		end,
	});
	HakiColorSection = ShopFrame:NewSection({
		Title = "Haki Color",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	HakiColorSection:NewToggle({
		Title = "Auto buy",
		Default = Configs.Shop.AutoBuyHakiColor,
		Callback = function(tr)
			Configs.Shop.AutoBuyHakiColor = tr;
		end,
	});
	AbilitySection = ShopFrame:NewSection({
		Title = "Abilities",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	AbilitySection:NewButton({
		Title = "Buy Geppo ($10,000)",
		Callback = function()
			CommF:InvokeServer("BuyHaki", "Geppo");
		end,
	});
	AbilitySection:NewButton({
		Title = "Buy Buso Haki ($25,000)",
		Callback = function()
			CommF:InvokeServer("BuyHaki", "Buso");
		end,
	});
	AbilitySection:NewButton({
		Title = "Buy Soru ($25,000)",
		Callback = function()
			CommF:InvokeServer("BuyHaki", "Soru");
		end,
	});
	AbilitySection:NewButton({
		Title = "Buy OBV Haki ($750,000)",
		Callback = function()
			CommF:InvokeServer("KenTalk", "Buy");
		end,
	});
	FightingStyleSe = ShopFrame:NewSection({
		Title = "Fighting Style",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	FightingStyleSe:NewButton({
		Title = "Black Leg ($150,000)",
		Callback = function()
			CommF:InvokeServer("BuyBlackLeg");
		end,
	});
	FightingStyleSe:NewButton({
		Title = "Buy Electro ($550,000)",
		Callback = function()
			CommF:InvokeServer("BuyElectro");
		end,
	});
	FightingStyleSe:NewButton({
		Title = "Buy Water Kung Fu ($750,000)",
		Callback = function()
			CommF:InvokeServer("BuyFishmanKarate");
		end,
	});
	FightingStyleSe:NewButton({
		Title = "Buy Dragon Claw (ƒ1,500 )",
		Callback = function()
			CommF:InvokeServer("BlackbeardReward", "DragonClaw", "1");
			CommF:InvokeServer("BlackbeardReward", "DragonClaw", "2");
		end,
	});
	FightingStyleSe:NewButton({
		Title = "Buy Superhuman ($3,000,000)",
		Callback = function()
			CommF:InvokeServer("BuySuperhuman");
		end,
	});
	FightingStyleSe:NewButton({
		Title = "Buy Death Step (ƒ5,000 $5,000,000)",
		Callback = function()
			CommF:InvokeServer("BuyDeathStep");
		end,
	});
	FightingStyleSe:NewButton({
		Title = "Buy Sharkman Karate (ƒ5,000 $2,500,000)",
		Callback = function()
			CommF:InvokeServer("BuySharkmanKarate", true);
			CommF:InvokeServer("BuySharkmanKarate");
		end,
	});
	FightingStyleSe:NewButton({
		Title = "Buy Electric Claw (ƒ5,000 $3,000,000)",
		Callback = function()
			CommF:InvokeServer("BuyElectricClaw");
		end,
	});
	FightingStyleSe:NewButton({
		Title = "Buy God Human (ƒ5,000 $5,000,000)",
		Callback = function()
			CommF:InvokeServer("BuyGodhuman");
		end,
	});
	FightingStyleSe:NewButton({
		Title = "Buy Sanguine Art (ƒ5,000 $5,000,000)",
		Callback = function()
			CommF:InvokeServer("BuySanguineArt", true);
			CommF:InvokeServer("BuySanguineArt");
		end,
	});




	SeaEventFrame = Windows:NewTab({
		Title = "Sea Events",
		Description = "",
		Icon = "rbxassetid://7743870134"
	});
	SeaEventEmber = SeaEventFrame:NewSection({
		Title = "Kitsune Island",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	SeaEventEmber:NewButton({
		Title = "Error getting Auto Azure Ember",
		Callback = function()
			
		end,
	});

	SeaEventMain = SeaEventFrame:NewSection({
		Title = "Sea Events Main (Unstackable)",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	SeaEventMain:NewToggle({
		Title = "Shark",
		Default = Configs.SeaEvents.Target.Shark,
		Callback = function(tr)
			Configs.SeaEvents.Target.Shark = tr;
		end,
	});
	SeaEventMain:NewToggle({
		Title = "Terrorshark",
		Default = Configs.SeaEvents.Target.Terrorshark,
		Callback = function(tr)
			Configs.SeaEvents.Target.Terrorshark = tr;
		end,
	});
	SeaEventMain:NewToggle({
		Title = "Piranha",
		Default = Configs.SeaEvents.Target.Piranha,
		Callback = function(tr)
			Configs.SeaEvents.Target.Piranha = tr;
		end,
	});
	SeaEventMain:NewToggle({
		Title = "Fish Crew Member",
		Default = Configs.SeaEvents.Target.FishCrewMember,
		Callback = function(tr)
			Configs.SeaEvents.Target.FishCrewMember = tr;
		end,
	});
	SeaEventMain:NewToggle({
		Title = "Auto Sail",
		Default = false,
		Callback = function(tr)
			Configs.System.Astaootslai = tr;
		end,
	});

	SeaEventShip = SeaEventFrame:NewSection({
		Title = "Ship",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	Selddctedvbaort = SeaEventShip:NewDropdown({
		Title = "Select Ship",
		Data = {"nil"},
		Default = "nil",
		Callback = function(a)
			Configs.System.SelectedShip = a;
		end,
	});
	SeaEventShip:NewButton({
		Title = "Refresh Ship",
		Callback = function()
			ReefeeshboatList = {};
			for i,v in pir(GetChildren(W.Boats)) do  
				tablein(ReefeeshboatList, v);
			end; twait();
			Selddctedvbaort.Set(ReefeeshboatList);
		end,
	});
	SeaEventShip:NewButton({
		Title = "Teleport To Selected Ship",
		Callback = function()
			Tp(Configs.System.SelectedShip.VehicleSeat.CFrame, Configs.Teleport.TP_Speed);
		end,
	});
	SeaEventShip:NewButton({
		Title = "Bring Ship",
		Callback = function()
			Tp2(Configs.System.SelectedShip.VehicleSeat.CFrame)
		end,
	});
	Section:NewToggle({
		Title = "Spectate Selected Ship",
		Default = false,
		Callback = function(tr)
			if tr then
				Cam.CameraSubject = Configs.System.SelectedShip.VehicleSeat;
			else
				Cam.CameraSubject = selc.Humanoid
			end;
		end,
	});



	local TabFrame = Windows:NewTab({
		Title = "Teleport",
		Description = "",
		Icon = "rbxassetid://7743870134"
	});
	local Section = TabFrame:NewSection({
		Title = "Teleport (I)",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	local aawdwa = Section:NewDropdown({
		Title = "Select Place",
		Data = {"nil"},
		Default = "nil",
		Callback = function(a)
			Configs.Teleport.Island.Target = a;
		end,
	});
	Section:NewButton({
		Title = "Fetch Island",
		Callback = function()
			IslFetcher = {};
			for _, v in pir(GetChildren(W._WorldOrigin.Locations)) do
				if v then
					tablein(IslFetcher, v);
				end;
			end; twait();
			aawdwa.Set(IslFetcher);
		end,
	});
	Section:NewButton({
		Title = "Teleport",
		Callback = function()
			if Configs.Teleport.TP_Bypass then
				TpBypass(Configs.Teleport.Island.Target:GetPivot() * CF(0,100,0));
				twait(0.7);
			end;
			Tp(Configs.Teleport.Island.Target:GetPivot() * CF(0,100,0), Configs.Teleport.TP_Speed or 2);
		end,
	});
	local Section = TabFrame:NewSection({
		Title = "Teleport (N)",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	local SNN = Section:NewDropdown({
		Title = "Select Nearest NPC",
		Data = {"nil"},
		Default = "nil",
		Callback = function(a)
			Configs.Teleport.NPCs.Target = a;
		end,
	});
	Section:NewButton({
		Title = "Refresh NPC",
		Callback = function()
			AllNPC = {};
			ALLNPCs = NPCs:GetChildren();
			for i=1, #ALLNPCs do
				tablein(AllNPC, ALLNPCs[i]);
			end; twait();
			SNN.Set(AllNPC);
		end,
	});
	Section:NewButton({
		Title = "Teleport",
		Callback = function()
			Tp(GetPivot(Configs.Teleport.NPCs.Target), Configs.Teleport.TP_Speed)
		end,
	});
	local Section = TabFrame:NewSection({
		Title = "Teleport (P)",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	local SNP = Section:NewDropdown({
		Title = "Select Player",
		Data = {"nil"},
		Default = "nil",
		Callback = function(a)
			Configs.Teleport.Player.Target = a;
		end,
	});
	Section:NewButton({
		Title = "Refresh Players",
		Callback = function()
			AllPlayer = {};
			ALLPlayer= P:GetChildren();
			for i=1, #ALLPlayer do
				tablein(AllPlayer, ALLPlayer[i]);
			end; twait();
			SNP.Set(AllPlayer);
		end,
	});
	Section:NewButton({
		Title = "Teleport",
		Callback = function()
			Tp(GetPivot(Configs.Teleport.Player.Target.Character), Configs.Teleport.TP_Speed);
		end,
	});
	local Section = TabFrame:NewSection({
		Title = "Teleport (B, M)",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	local SNMB = Section:NewDropdown({
		Title = "Select Nearest Mob/Boss",
		Data = {"nil"},
		Default = "nil",
		Callback = function(a)
			Configs.Teleport.Mob_Boss.Target = a;
		end,
	});
	Section:NewButton({
		Title = "Refresh Mob/Boos",
		Callback = function()
			AllMobOrBoss = {};
			ALLMobOrBoss = Enem;
			for i=1, #ALLMobOrBoss do
				tablein(AllMobOrBoss, ALLMobOrBoss[i]);
			end; twait();
			SNMB.Set(AllMobOrBoss);
		end,
	});
	Section:NewButton({
		Title = "Teleport",
		Callback = function()
			Tp(GetPivot(Configs.Teleport.Mob_Boss.Target), Configs.Teleport.TP_Speed);
		end,
	});
	WorldTeleportSection = TabFrame:NewSection({
		Title = "Teleport (W)",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	WorldTeleportSection:NewButton({
		Title = "Sea 1",
		Callback = function()
			CommF:InvokeServer("TravelMain");
		end,
	});
	WorldTeleportSection:NewButton({
		Title = "Sea 2",
		Callback = function()
			CommF:InvokeServer("TravelDressrosa");
		end,
	});
	WorldTeleportSection:NewButton({
		Title = "Sea 3",
		Callback = function()
			CommF:InvokeServer("TravelZou");
		end,
	});



	ESPFrame = Windows:NewTab({
		Title = "ESP",
		Description = "",
		Icon = "rbxassetid://7743870134"
	});
	ESPSection = ESPFrame:NewSection({
		Title = "ESP",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	ESPSection:NewToggle({
		Title = "Flower",
		Default = Configs.ESP.Flower,
		Callback = function(tr)
			Configs.ESP.Flower = tr;
		end,
	});
	ESPSection:NewToggle({
		Title = "Fruits",
		Default = Configs.ESP.Fruits,
		Callback = function(tr)
			Configs.ESP.Fruits = tr;
		end,
	});
	ESPSection:NewToggle({
		Title = "Players",
		Default = Configs.ESP.Players,
		Callback = function(tr)
			Configs.ESP.Players = tr;
		end,
	});
	ESPSection:NewToggle({
		Title = "Chests",
		Default = Configs.ESP.Chests,
		Callback = function(tr)
			Configs.ESP.Chests = tr;
		end,
	});
	ESPSection:NewToggle({
		Title = "IslandESP",
		Default = Configs.ESP.IslandESP,
		Callback = function(tr)
			Configs.ESP.IslandESP = tr;
		end,
	});
	ESPSection:NewToggle({
		Title = "Real Fruits",
		Default = Configs.ESP.RealFruits,
		Callback = function(tr)
			Configs.ESP.RealFruits = tr;
		end,
	});
	ESPSection:NewToggle({
		Title = "Mirage Island",
		Default = Configs.ESP.Mirage,
		Callback = function(tr)
			Configs.ESP.Mirage = tr;
		end,
	});
	ESPSection:NewToggle({
		Title = "Advanced Fruit Dealer",
		Default = Configs.ESP.AdvancedFD,
		Callback = function(tr)
			Configs.ESP.AdvancedFD = tr;
		end,
	});
	ESPSection:NewToggle({
		Title = "Gear",
		Default = Configs.ESP.Gear,
		Callback = function(tr)
			Configs.ESP.Gear = tr;
		end,
	});
	ESPSection:NewToggle({
		Title = "Show_Script_Service_Pack",
		Default = Configs.ESP.S3W2,
		Callback = function(tr)
			Configs.ESP.S3W2 = tr;
		end,
	});



	local TabFrame = Windows:NewTab({
		Title = "Server & Client",
		Description = "",
		Icon = "rbxassetid://7743870134"
	});
	local Section = TabFrame:NewSection({
		Title = "Server",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	Section:NewToggle({
		Title = "Auto Execute",
		Default = false,
		Callback = function(tr)
			Configs.Server.AutoExec = tr;
		end,
	});
	Section:NewButton({
		Title = "Rejoin",
		Callback = function()
			if Configs.Server.AutoExec then
				AutoExec();
			end;
			T:TeleportToPlaceInstance(placeId, game.JobId, selff);
		end,
	});
	Section:NewButton({
		Title = "Hop",
		Callback = function()
			if Configs.Server.AutoExec then
				AutoExec();
			end;
			local module = (loadstring(game:HttpGet("https://roblox.relzscript.xyz/Hop.lua")))();
			module:Teleport(placeId, "Singapore");
		end,
	});
	Section:NewButton({
		Title = "Hop (2)",
		Callback = function()
			if Configs.Server.AutoExec then
				AutoExec();
			end;
			T:Teleport(placeId, selff);
		end,
	});
	Section:NewTitle("Place Id: " .. tos(placeId))
	RepText(Section, 20, "Server JobId: " .. tos(game.JobId));
	Section:NewButton({
		Title = "Copy Server JobId",
		Callback = function()
			setc(game.JobId);
		end,
	});

	ClientRequest = TabFrame:NewSection({
		Title = "Reuqest",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	ClientRequest:NewTextbox({
		Title = "Webhook",
		Default = "https://NeuronXOn.Top/Stack",
		FileType = "",
		Callback = function(a)
			Configs.System.WebhookRequest = a;
		end,
	});
	ClientRequest:NewButton({
		Title = "Open Roblox Console",
		Callback = function()
			C.DevConsoleMaster.Enabled = not C.DevConsoleMaster.Enabled
		end,
	});
	HasLoadExtraAsset = false;
	ClientRequest:NewButton({
		Title = "Load Extra Asset",
		Callback = function()
			HasLoadExtraAsset = true;
			Lib = loadstring(game:HttpGet('https://raw.githubusercontent.com/Yumiara/Python/refs/heads/main/NeuronX.py'))();
			Notification.new({
				Description = "Dashboard keybind is \\";
				Title = "Neuron X [Guide]";
				Duration = 3;
			});
		end,
	});
	ClientRequest:NewButton({
		Title = "Toggle Dashboard",
		Callback = function()
			if HasLoadExtraAsset then
				if Lib then
					getgenv().NeuDa();
				else
					Lib = loadstring(game:HttpGet('https://raw.githubusercontent.com/Yumiara/Python/refs/heads/main/NeuronX.py'))();
				end;
			else
				Notification.new({
					Description = "Please load extra asset before open dashboard";
					Title = "Neuron X [Failed]";
					Duration = 3;
				});
			end;
		end,
	});
	ClientRequest:NewButton({
		Title = "Request Inventory (board)",
		Callback = function()
			InventoryRequestData = ("Inventory = " .. tableTostring(CommF:InvokeServer("getInventory"), 1) .. ";");
			Lib:SendRequest(InventoryRequestData);
		end,
	});
	ClientRequest:NewButton({
		Title = "Request Fruits (board)",
		Callback = function()
			InventoryRequestData = ("FruitShop = " .. tableTostring(CommF:InvokeServer("GetFruits"), 1) .. ";");
			Lib:SendRequest(InventoryRequestData);
		end,
	});





	ClientInfo = TabFrame:NewSection({
		Title = "Client",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	ActiveTimeT = RepText(ClientInfo, 20, "Active Time: " .. tos(GG.ActiveTime));
	PhysicsFPST = RepText(ClientInfo, 20, "Physics FPS: " .. tos(W:GetRealPhysicsFPS()));
	ClientFPST = RepText(ClientInfo, 20, "Attempting to find");
	PingT = RepText(ClientInfo, 20, "Ping: ".. (game:GetService("Stats")).Network.ServerStatsItem["Data Ping"]:GetValueString());
	ExecutorT = RepText(ClientInfo, 20, "Executor: " .. Iden);
	UNCPreparedAsT = RepText(ClientInfo, 20, "UNC Prepared: " .. Iden);


	ClientDebug = TabFrame:NewSection({
		Title = "Client Debug",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	IsselcT = RepText(ClientDebug, 20, "Is Character: " .. tos(Isselc));
	MuchBackpackT = RepText(ClientDebug, 20, "Backpack: " .. tos(Backpack and #GetChildren(Backpack)));
	AnyStatsT = RepText(ClientDebug, 20, "AnyStats: " .. tos(AnyStats));
	AnySea1T = RepText(ClientDebug, 20, "AnySea1: " .. tos(AnySea1));
	AnySea2T = RepText(ClientDebug, 20, "AnySea2: " .. tos(AnySea2));
	AnySea3T = RepText(ClientDebug, 20, "AnySea3: " .. tos(AnySea3));
	AnySea1WT = RepText(ClientDebug, 20, "AnySea1WT: " .. tos(AnySea1W));
	AnySea2WT = RepText(ClientDebug, 20, "AnySea2WT: " .. tos(AnySea2W));
	AnySea3WT = RepText(ClientDebug, 20, "AnySea3WT: " .. tos(AnySea3W));
	NeedAttackingT = RepText(ClientDebug, 20, "NeedAttacking: " .. tos(NeedAttacking));
	MuchEnemT = RepText(ClientDebug, 20, "Enem: " .. tos(Enem and #Enem or 0));
	CharsT = RepText(ClientDebug, 20, "Chars: " .. tos(Chars and #Chars or 0));
	ClientDebug:NewButton({
		Title = "Debug FindBoat",
		Callback = function()
			Notification.new({
				Description = tostring(FindBoat());
				Title = "Neuron X [Debug]";
				Duration = 5;
			});
		end,
	});



	local TabFrame = Windows:NewTab({
		Title = "Settings",
		Description = "Full Access",
		Icon = "rbxassetid://7733960981"
	});

	local Section = TabFrame:NewSection({
		Title = "Mobile Fly Settings",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});

	Section:NewToggle({
		Title = "Mobile Fly Button (Show/Hide)",
		Default = false,
		Callback = function(tr)
			if not tr then
				TTJYMobileIn.W.Visible = false; TTJYMobileIn.A.Visible = false; TTJYMobileIn.S.Visible = false; TTJYMobileIn.D.Visible = false;
			else
				TTJYMobileIn.W.Visible = true; TTJYMobileIn.A.Visible = true; TTJYMobileIn.S.Visible = true; TTJYMobileIn.D.Visible = true;
			end;
		end,
	});

	TTJYMobileIn.W.Visible = false;
	TTJYMobileIn.A.Visible = false;
	TTJYMobileIn.S.Visible = false;
	TTJYMobileIn.D.Visible = false;

	Section:NewToggle({
		Title = "Mobile Fly Button (Up/Down)",
		Default = false,
		Callback = function(tr)
			if not tr then
				TTJYMobileIn.W.Position = UDim2.new(0.05, 0, 0.1, 0); TTJYMobileIn.A.Position = UDim2.new(0, 0, 0.2, 0) TTJYMobileIn.S.Position = UDim2.new(0.05, 0, 0.3, 0); TTJYMobileIn.D.Position = UDim2.new(0.1, 0, 0.2, 0)
			else
				TTJYMobileIn.W.Position = UDim2.new(0.05, 0, 0.7, 0); TTJYMobileIn.A.Position = UDim2.new(0, 0, 0.8, 0) TTJYMobileIn.S.Position = UDim2.new(0.05, 0, 0.9, 0); TTJYMobileIn.D.Position = UDim2.new(0.1, 0, 0.8, 0)
			end;
		end,
	});

	Section:NewToggle({
		Title = "Mobile Fly Button (Big/Small)",
		Default = false,
		Callback = function(tr)
			if not tr then
				TTJYMobileIn.W.Size = UDim2.new(0.1, 0, 0.1, 0); TTJYMobileIn.A.Size = UDim2.new(0.1, 0, 0.1, 0) TTJYMobileIn.S.Size = UDim2.new(0.1, 0, 0.1, 0); TTJYMobileIn.D.Size = UDim2.new(0.1, 0, 0.1, 0)
			else
				TTJYMobileIn.W.Size = UDim2.new(0.05, 0, 0.05, 0); TTJYMobileIn.A.Size = UDim2.new(0.05, 0, 0.05, 0) TTJYMobileIn.S.Size = UDim2.new(0.05, 0, 0.05, 0); TTJYMobileIn.D.Size = UDim2.new(0.05, 0, 0.05, 0)
			end;
		end,
	});

	local Section = TabFrame:NewSection({
		Title = "Auto Farm Settings",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});

	Section:NewToggle({
		Title = "Hitbox Expansion",
		Default = Configs.Settings.AutoFarm.Hitbox,
		Callback = function(tr)
			Configs.Settings.AutoFarm.Hitbox = tr;
		end,
	});
	Section:NewToggle({
		Title = "Hitbox Expansion Aura",
		Default = false,
		Callback = function(tr)
			tspawn(function()
				while tr do twait();
					for _, v in pir(Enem) do
						if FindFirstChild(v, "HumanoidRootPart") then
							v.HumanoidRootPart.Size = Vec3(50, 50, 50); twait();
						end;
					end;
				end;
			end);
		end,
	});

	Section:NewDropdown({
		Title = "Bring Mob Type",
		Data = {"Target", "Any"},
		Default = "Target",
		Callback = function(a)
			Configs.Settings.AutoFarm.BringMobType = a;
		end,
	});
	Section:NewToggle({
		Title = "Bring Mob",
		Default = Configs.Settings.AutoFarm.BringMob,
		Callback = function(tr)
			Configs.Settings.AutoFarm.BringMob = tr;
		end,
	});
	Section:NewSlider({
		Title = "Bring Mob Distance",
		Min = 1,
		Max = 630,
		Default = Configs.Settings.AutoFarm.BringMobDistance,
		Callback = function(a)
			Configs.Settings.AutoFarm.BringMobDistance = a;
		end,
	});

	Stackable = Section:NewToggle({
		Title = "Stackable (Turn on everything without any error)",
		Default = true,
		Callback = function(tr)
			if not tr then
				return Notification.new({
					Description = "Failed to Stack (We force enable this)";
					Title = "Neuron X [Error]";
					Duration = 10;
				});
			end;
			Configs.Settings.AutoFarm.Stackable = true;
			Stackable.Value(true);
			Stackable.Visible(true);
		end,
	});

	Section:NewSlider({
		Title = "Position X",
		Min = -50,
		Max = 50,
		Default = Configs.Settings.AutoFarm.PosX,
		Callback = function(a)
			Configs.Settings.AutoFarm.PosX = a;
		end,
	})
	Section:NewSlider({
		Title = "Position Y",
		Min = -50,
		Max = 50,
		Default = Configs.Settings.AutoFarm.PosY,
		Callback = function(a)
			Configs.Settings.AutoFarm.PosY = a;
		end,
	})
	Section:NewSlider({
		Title = "Position Z",
		Min = -50,
		Max = 50,
		Default = Configs.Settings.AutoFarm.PosZ,
		Callback = function(a)
			Configs.Settings.AutoFarm.PosZ = a;
		end,
	})
	Section:NewSlider({
		Title = "Teleport Speed",
		Min = 1,
		Max = 10,
		Default = Configs.Settings.AutoFarm.TP_Speed,
		Callback = function(a)
			Configs.Settings.AutoFarm.TP_Speed = a;
		end,
	})

	local Section = TabFrame:NewSection({
		Title = "Auto Raid Settings",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});

	Section:NewSlider({
		Title = "Position X",
		Min = -50,
		Max = 50,
		Default = Configs.Settings.AutoRaid.PosX,
		Callback = function(a)
			Configs.Settings.AutoRaid.PosX = a;
		end,
	})
	Section:NewSlider({
		Title = "Position Y",
		Min = -50,
		Max = 50,
		Default = Configs.Settings.AutoRaid.PosY,
		Callback = function(a)
			Configs.Settings.AutoRaid.PosY = a;
		end,
	})
	Section:NewSlider({
		Title = "Position Z",
		Min = -50,
		Max = 50,
		Default = Configs.Settings.AutoRaid.PosZ,
		Callback = function(a)
			Configs.Settings.AutoRaid.PosZ = a;
		end,
	})
	Section:NewSlider({
		Title = "Teleport Speed",
		Min = 1,
		Max = 10,
		Default = Configs.Settings.AutoRaid.TP_Speed,
		Callback = function(a)
			Configs.Settings.AutoRaid.TP_Speed = a;
		end,
	})

	local Section = TabFrame:NewSection({
		Title = "Script System Setting",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});

	Section:NewToggle({
		Title = "Client Debug",
		Default = false,
		Callback = function(tr)
			
		end,
	});
	Section:NewToggle({
		Title = "Use Commands",
		Default = false,
		Callback = function(tr)
			
		end,
	});
	Section:NewButton({
		Title = "Destroy System",
		Callback = function()
			
		end,
	});

	local Section = TabFrame:NewSection({
		Title = "Auto Bounty Settings",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});

	Section:NewSlider({
		Title = "Position X",
		Min = -50,
		Max = 50,
		Default = Configs.Settings.AutoBounty.PosX,
		Callback = function(a)
			Configs.Settings.AutoBounty.PosX = a;
		end,
	})
	Section:NewSlider({
		Title = "Position Y",
		Min = -50,
		Max = 50,
		Default = Configs.Settings.AutoBounty.PosY,
		Callback = function(a)
			Configs.Settings.AutoBounty.PosY = a;
		end,
	})
	Section:NewSlider({
		Title = "Position Z",
		Min = -50,
		Max = 50,
		Default = Configs.Settings.AutoBounty.PosZ,
		Callback = function(a)
			Configs.Settings.AutoBounty.PosZ = a;
		end,
	})
	Section:NewSlider({
		Title = "Teleport Speed",
		Min = 1,
		Max = 10,
		Default = Configs.Settings.AutoBounty.TP_Speed,
		Callback = function(a)
			Configs.Settings.AutoBounty.TP_Speed = a;
		end,
	})

	local Section = TabFrame:NewSection({
		Title = "Auto Chest Settings",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	Section:NewToggle({
		Title = "Detail (Can be use with Normal)",
		Default = Configs.Settings.AutoChest.Detail,
		Callback = function(tr)
			Configs.Settings.AutoChest.Detail = tr;
		end,
	});
	Section:NewToggle({
		Title = "Normal (Can be use with Detail)",
		Default = Configs.Settings.AutoChest.Normal,
		Callback = function(tr)
			Configs.Settings.AutoChest.Normal = tr;
		end,
	});
	Section:NewToggle({
		Title = "Fragment Chest (Always True)",
		Default = Configs.Settings.AutoChest.Fragment,
		Callback = function(tr)
			Configs.Settings.AutoChest.Fragment = true;
			Notification.new({
				Description = "Always True, can't be turn off";
				Title = "Neuron X [Return]";
				Duration = 3;
			});
		end,
	});
	Section:NewSlider({
		Title = "Teleport Speed",
		Min = 1,
		Max = 10,
		Default = Configs.Settings.AutoChest.TP_Speed,
		Callback = function(a)
			Configs.Settings.AutoChest.TP_Speed = a;
		end,
	})

	local Section = TabFrame:NewSection({
		Title = "Teleport Setting",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	Section:NewToggle({
		Title = "TP Bypass",
		Default = Configs.Teleport.TP_Bypass,
		Callback = function(tr)
			Configs.Teleport.TP_Bypass = tr;
		end,
	});
	Section:NewSlider({
		Title = "Teleport Speed",
		Min = 1,
		Max = 10,
		Default = Configs.Teleport.TP_Speed,
		Callback = function(a)
			Configs.Teleport.TP_Speed = a;
		end,
	})
	local Section = TabFrame:NewSection({
		Title = "Game",
		Icon = "rbxassetid://7743869054",
		Position = "Right"
	});
	Section:NewSlider({
		Title = "Gravity",
		Min = 0,
		Max = 600,
		Default = 192,
		Callback = function(a)
			W.Gravity = a;
		end,
	});
	Section:NewDropdown({
		Title = "Lighting Technology",
		Data ={"Compatibility"; "Future";
			"Legacy"; "ShadowMap";
			"Unified"; "Voxel"
		},
		Default = "ShadowMap",
		Callback = function(a)
			L.Technology = Enum.Technology[a];
		end,
	});
	SeaEventSettings = TabFrame:NewSection({
		Title = "Sea Events",
		Icon = "rbxassetid://7743869054",
		Position = "Left"
	});
	SeaEventSettings:NewSlider({
		Title = "Ship speed",
		Min = 1,
		Max = 12,
		Default = Configs.SeaEvents.TP_Speed,
		Callback = function(a)
			Configs.SeaEvents.TP_Speed = a;
		end,
	});


	-- UI



	-- Functions

	AlwaysNameESP = "!2\n!\n?Protectec?NeuronX?By?\n+%\n*-"

	function ClearESP()
		for _, v in pir(GG["ESPT2"]) do
			v:Destroy(true);
		end;
	end;

	function round(n)
		return math.floor(tonumber(n) + 0.5);
	end;

	function UpdateP()
		for i, v in pir(GetChildren(P)) do
			pcal(function()
				if v and v.Character then
					if Configs.ESP.Players then
						if v and v.Character and FindFirstChild(v.Character, "Head") and not FindFirstChild(v.Character.Head, AlwaysNameESP) then
							billP = Instancen("BillboardGui", v.Character.Head);
							billP.Name = AlwaysNameESP;
							billP.ExtentsOffset = Vector3.new(0, 1, 0);
							billP.Size = UDim2.new(1, 200, 1, 30);
							billP.Adornee = v.Character.Head;
							billP.AlwaysOnTop = true;
							nameP = Instancen("TextLabel", billP);
							nameP.Font = Enum.Font.GothamSemibold;
							nameP.FontSize = "Size14";
							nameP.TextWrapped = true;
							nameP.Text = v.Name .. " \n" .. round(dist(v.Character.Head.Position) / 3) .. " Distance";
							nameP.Size = UDim2.new(1, 0, 1, 0);
							nameP.TextYAlignment = "Top";
							nameP.BackgroundTransparency = 1;
							nameP.TextStrokeTransparency = 0.5;
							nameP.TextSize = 12;
							if v.Team == selff.Team then
								nameP.TextColor3 = Color3.new(0, 255, 0);
							else
								nameP.TextColor3 = Color3.new(255, 0, 0);
							end;
							tablein(GG["ESPT2"], billP);
						else
							v.Character.Head[AlwaysNameESP].TextLabel.Text = v.Name .. " | " .. round(dist(v.Character.Head.Position) / 3) .. " Distance\nHealth : " .. round(v.Character.Humanoid.Health * 100 / v.Character.Humanoid.MaxHealth) .. "%";
						end;
					elseif FindFirstChild(v.Character.Head, AlwaysNameESP) then
						FindFirstChild(v.Character.Head, AlwaysNameESP):Destroy(true);
					end;
				end;
			end);
		end;
	end;
	function UpdateESPOver()
		for i, v in pir(GetChildren(W)) do
			pcal(function()
				if strfind(v.Name, "Chest") then
					if Configs.ESP.Chests then
						if not FindFirstChild(v, AlwaysNameESP) then
							billC = Instance.new("BillboardGui", v);
							billC.Name = AlwaysNameESP;
							billC.ExtentsOffset = Vector3.new(0, 1, 0);
							billC.Size = UDim2.new(1, 200, 1, 30);
							billC.Adornee = v;
							billC.AlwaysOnTop = true;
							nameC = Instance.new("TextLabel", billC);
							nameC.Font = Enum.Font.GothamSemibold;
							nameC.FontSize = "Size14";
							nameC.TextWrapped = true;
							nameC.Size = UDim2.new(1, 0, 1, 0);
							nameC.TextYAlignment = "Top";
							nameC.BackgroundTransparency = 1;
							nameC.TextStrokeTransparency = 0.5;
							nameC.TextSize = 12;
							if v.Name == "Chest1" then
								nameC.TextColor3 = Color3.fromRGB(109, 109, 109);
								nameC.Text = "Chest 1" .. " \n" .. round(dist(v.Position) / 3) .. " Distance";
							end;
							if v.Name == "Chest2" then
								nameC.TextColor3 = Color3.fromRGB(173, 158, 21);
								nameC.Text = "Chest 2" .. " \n" .. round(dist(v.Position) / 3) .. " Distance";
							end;
							if v.Name == "Chest3" then
								nameC.TextColor3 = Color3.fromRGB(85, 255, 255);
								nameC.Text = "Chest 3" .. " \n" .. round(dist(v.Position) / 3) .. " Distance";
							end;
						else
							v[AlwaysNameESP].TextLabel.Text = v.Name .. "   \n" .. round(dist(v.Position) / 3) .. " Distance";
						end;
					elseif FindFirstChild(v, AlwaysNameESP) then
						FindFirstChild(v, AlwaysNameESP):Destroy(true);
					end;
				end;
			end);
			pcal(function()
				if strfind(v.Name, "Fruit") then
					if Configs.ESP.Fruits then
						if not FindFirstChild(v, AlwaysNameESP) then
							billFr = Instance.new("BillboardGui", v);
							billFr.Name = AlwaysNameESP;
							billFr.ExtentsOffset = Vector3.new(0, 1, 0);
							billFr.Size = UDim2.new(1, 200, 1, 30);
							billFr.Adornee = v;
							billFr.AlwaysOnTop = true;
							nameC = Instance.new("TextLabel", billFr);
							nameC.Font = Enum.Font.GothamSemibold;
							nameC.FontSize = "Size14";
							nameC.TextWrapped = true;
							nameC.Size = UDim2.new(1, 0, 1, 0);
							nameC.TextYAlignment = "Top";
							nameC.BackgroundTransparency = 1;
							nameC.TextStrokeTransparency = 0.5;
							nameC.TextSize = 12;
							nameC.TextColor3 = Color3.fromRGB(255, 255, 255);
							nameC.Text = v.Name .. " \n" .. round(dist(v.Handle.Position) / 3) .. " Distance";
						else
							v[AlwaysNameESP].TextLabel.Text = v.Name .. " \n" .. round(dist(v.Handle.Position) / 3) .. " Distance";
						end;
					elseif FindFirstChild(v, AlwaysNameESP) then
						FindFirstChild(v, AlwaysNameESP):Destroy(true);
					end;
				end;
			end);
			pcal(function()
				if v.Name == "Flower2" or v.Name == "Flower1" then
					if Configs.ESP.Flower then
						if not FindFirstChild(v, AlwaysNameESP) then
							billFl = Instance.new("BillboardGui", v);
							billFl.Name = AlwaysNameESP;
							billFl.ExtentsOffset = Vector3.new(0, 1, 0);
							billFl.Size = UDim2.new(1, 200, 1, 30);
							billFl.Adornee = v;
							billFl.AlwaysOnTop = true;
							nameFl = Instance.new("TextLabel", billFl);
							nameFl.Font = Enum.Font.GothamSemibold;
							nameFl.FontSize = "Size14";
							nameFl.TextWrapped = true;
							nameFl.Size = UDim2.new(1, 0, 1, 0);
							nameFl.TextYAlignment = "Top";
							nameFl.BackgroundTransparency = 1;
							nameFl.TextStrokeTransparency = 0.5;
							nameFl.TextSize = 12;
							nameFl.TextColor3 = Color3.fromRGB(255, 255, 255);
							if v.Name == "Flower1" then
								nameFl.Text = "Flower" .. " \n" .. round(dist(v.Position) / 3) .. " Distance";
								nameFl.TextColor3 = Color3.fromRGB(0, 0, 255);
							end;
							if v.Name == "Flower2" then
								nameFl.Text = "Flower" .. " \n" .. round(dist(v.Position) / 3) .. " Distance";
								nameFl.TextColor3 = Color3.fromRGB(255, 0, 0);
							end;
						else
							v[AlwaysNameESP].TextLabel.Text = v.Name .. " \n" .. round(dist(v.Position) / 3) .. " Distance";
						end;
					elseif FindFirstChild(v, AlwaysNameESP) then
						FindFirstChild(v, AlwaysNameESP):Destroy(true);
					end;
				end;
			end);
		end;
	end;
	function UpdateRLFr()
		if WorldCheck["Third Sea"] then
			EachAAA = W.AppleSpawner or nil;
			EachABB = W.PineappleSpawner or nil;
			EachACC = W.BananaSpawner or nil;

			FromEachAAA = EachAAA and FindFirstChildWhichIsA(EachAAA, "Tool");
			FromEachABB = EachABB and FindFirstChildWhichIsA(EachABB, "Tool");
			FromEachACC = EachACC and FindFirstChildWhichIsA(EachACC, "Tool");

			if FromEachAAA then
				pcal(function()
					if Configs.ESP.RealFruits and not FindFirstChild(FromEachAAA.Handle, AlwaysNameESP) then
						billFromEachAAA = Instance.new("BillboardGui", FromEachAAA.Handle);
						billFromEachAAA.Name = AlwaysNameESP;
						billFromEachAAA.ExtentsOffset = Vector3.new(0, 1, 0);
						billFromEachAAA.Size = UDim2.new(1, 200, 1, 30);
						billFromEachAAA.Adornee = v;
						billFromEachAAA.AlwaysOnTop = true;
						nameFromEachAAA = Instance.new("TextLabel", billFromEachAAA);
						nameFromEachAAA.Font = Enum.Font.GothamSemibold;
						nameFromEachAAA.FontSize = "Size14";
						nameFromEachAAA.TextWrapped = true;
						nameFromEachAAA.Size = UDim2.new(1, 0, 1, 0);
						nameFromEachAAA.TextYAlignment = "Top";
						nameFromEachAAA.BackgroundTransparency = 1;
						nameFromEachAAA.TextStrokeTransparency = 0.5;
						nameFromEachAAA.TextSize = 11;
						nameFromEachAAA.TextColor3 = Color3.fromRGB(255, 0, 0);
						nameFromEachAAA.Text = "Apple";
					else
						if not Configs.ESP.RealFruits and FindFirstChild(FromEachAAA.Handle, AlwaysNameESP) then
							FindFirstChild(FromEachAAA.Handle, AlwaysNameESP):Destroy(true);
						end;
					end;
				end);
			end;
			if FromEachABB then
				pcal(function()
					if Configs.ESP.RealFruits and not FindFirstChild(FromEachABB.Handle, AlwaysNameESP) then
						billFromEachABB = Instance.new("BillboardGui", FromEachABB.Handle);
						billFromEachABB.Name = AlwaysNameESP;
						billFromEachABB.ExtentsOffset = Vector3.new(0, 1, 0);
						billFromEachABB.Size = UDim2.new(1, 200, 1, 30);
						billFromEachABB.Adornee = v;
						billFromEachABB.AlwaysOnTop = true;
						nameFromEachABB = Instance.new("TextLabel", billFromEachABB);
						nameFromEachABB.Font = Enum.Font.GothamSemibold;
						nameFromEachABB.FontSize = "Size14";
						nameFromEachABB.TextWrapped = true;
						nameFromEachABB.Size = UDim2.new(1, 0, 1, 0);
						nameFromEachABB.TextYAlignment = "Top";
						nameFromEachABB.BackgroundTransparency = 1;
						nameFromEachABB.TextStrokeTransparency = 0.5;
						nameFromEachABB.TextSize = 11;
						nameFromEachABB.TextColor3 = Color3.fromRGB(255, 174, 0);
						nameFromEachABB.Text = "Pineapple";
					else
						if not Configs.ESP.RealFruits and FindFirstChild(FromEachABB.Handle, AlwaysNameESP) then
							FindFirstChild(FromEachABB.Handle, AlwaysNameESP):Destroy(true);
						end;
					end;
				end);
			end;
			if FromEachACC then
				pcal(function()
					if Configs.ESP.RealFruits and not FindFirstChild(FromEachACC.Handle, AlwaysNameESP) then
						billFromEachACC = Instance.new("BillboardGui", FromEachACC.Handle);
						billFromEachACC.Name = AlwaysNameESP;
						billFromEachACC.ExtentsOffset = Vector3.new(0, 1, 0);
						billFromEachACC.Size = UDim2.new(1, 200, 1, 30);
						billFromEachACC.Adornee = v;
						billFromEachACC.AlwaysOnTop = true;
						nameFromEachACC = Instance.new("TextLabel", billFromEachACC);
						nameFromEachACC.Font = Enum.Font.GothamSemibold;
						nameFromEachACC.FontSize = "Size14";
						nameFromEachACC.TextWrapped = true;
						nameFromEachACC.Size = UDim2.new(1, 0, 1, 0);
						nameFromEachACC.TextYAlignment = "Top";
						nameFromEachACC.BackgroundTransparency = 1;
						nameFromEachACC.TextStrokeTransparency = 0.5;
						nameFromEachACC.TextSize = 11;
						nameFromEachACC.TextColor3 = Color3.fromRGB(251, 255, 0);
						nameFromEachACC.Text = "Banana";
					else
						if not Configs.ESP.RealFruits and FindFirstChild(FromEachACC.Handle, AlwaysNameESP) then
							FindFirstChild(FromEachACC.Handle, AlwaysNameESP):Destroy(true);
						end;
					end;
				end);
			end;
		end;
	end;
	function UpdateIsland()
		for i, v in pir(GetChildren(W._WorldOrigin.Locations)) do
			pcal(function()
				if Configs.ESP.IslandESP then
					if v.Name ~= "Sea" then
						if not FindFirstChild(v, AlwaysNameESP) then
							billIsl = Instancen("BillboardGui", v);
							billIsl.Name = AlwaysNameESP;
							billIsl.ExtentsOffset = Vector3.new(0, 1, 0);
							billIsl.Size = UDim2.new(1, 200, 1, 30);
							billIsl.Adornee = v;
							billIsl.AlwaysOnTop = true;
							nameIsl = Instancen("TextLabel", billIsl);
							nameIsl.Font = "GothamBold";
							nameIsl.FontSize = "Size14";
							nameIsl.TextWrapped = true;
							nameIsl.Size = UDim2.new(1, 0, 1, 0);
							nameIsl.TextYAlignment = "Top";
							nameIsl.BackgroundTransparency = 1;
							nameIsl.TextStrokeTransparency = 0.5;
							nameIsl.TextColor3 = Color3.fromRGB(7, 236, 240);
							nameIsl.TextSize = 12;
							nameIsl.Text = v.Name .. " \n" .. round(dist(v.Position) / 3) .. " Distance";
						else
							v[AlwaysNameESP].TextLabel.Text = v.Name .. " \n" .. round(dist(v.Position) / 3) .. " Distance";
						end;
					end;
				elseif FindFirstChild(v, AlwaysNameESP) then
					FindFirstChild(v, AlwaysNameESP):Destroy(true);
				end;
			end);
		end;
	end;
	function UpdateG()
		if FindFirstChild(W.Map, "MysticIsland") then
			GearPathByGet = FindFirstChild(W.Map.MysticIsland, "MeshPart")
			if GearPathByGet and not FindFirstChild(GearPathByGet, AlwaysNameESP) then
				if Configs.ESP.Gear then
					billIsl = Instancen("BillboardGui", GearPathByGet);
					billIsl.Name = AlwaysNameESP;
					billIsl.ExtentsOffset = Vector3.new(0, 1, 0);
					billIsl.Size = UDim2.new(1, 200, 1, 30);
					billIsl.Adornee = GearPathByGet;
					billIsl.AlwaysOnTop = true;
					nameIsl = Instancen("TextLabel", billIsl);
					nameIsl.Font = "GothamBold";
					nameIsl.FontSize = "Size14";
					nameIsl.TextWrapped = true;
					nameIsl.Size = UDim2.new(1, 0, 1, 0);
					nameIsl.TextYAlignment = "Top";
					nameIsl.BackgroundTransparency = 1;
					nameIsl.TextStrokeTransparency = 0.5;
					nameIsl.TextColor3 = Color3.fromRGB(80, 245, 245);
					nameIsl.TextSize = 12;
					nameIsl.Text = GearPathByGet.Name .. " \n" .. round(dist(GearPathByGet.Position) / 3) .. " Distance";
				end;
			elseif GearPathByGet and FindFirstChild(GearPathByGet, AlwaysNameESP) then
				if Configs.ESP.Gear then
					FindFirstChild(GearPathByGet, AlwaysNameESP).TextLabel.Text = "Gear" .. " \n" .. round(dist(v.Position) / 3) .. " Distance";
				else
					FindFirstChild(GearPathByGet, AlwaysNameESP):Destroy(true);
				end;
			end;
		end;
	end;
	function UpdateAFD()
		for i, v in pir(GetChildren(W.NPCs)) do
			pcal(function()
				if Configs.ESP.AdvancedFD then
					if v.Name == "Advanced Fruit Dealer" then
						if not FindFirstChild(v, AlwaysNameESP) then
							billAFD = Instance.new("BillboardGui", v);
							billAFD.Name = AlwaysNameESP;
							billAFD.ExtentsOffset = Vector3.new(0, 1, 0);
							billAFD.Size = UDim2.new(1, 200, 1, 30);
							billAFD.Adornee = v;
							billAFD.AlwaysOnTop = true;
							nameAFD = Instance.new("TextLabel", billAFD);
							nameAFD.Font = "Code";
							nameAFD.FontSize = "Size14";
							nameAFD.TextWrapped = true;
							nameAFD.Size = UDim2.new(1, 0, 1, 0);
							nameAFD.TextYAlignment = "Top";
							nameAFD.BackgroundTransparency = 1;
							nameAFD.TextStrokeTransparency = 0.5;
							nameAFD.TextColor3 = Color3.fromRGB(80, 245, 245);
							nameAFD.TextSize = 12;
						else
							v[AlwaysNameESP].TextLabel.Text = v.Name .. " \n" .. round(dist(v.Position) / 3) .. " M";
						end;
					end;
				elseif FindFirstChild(v, AlwaysNameESP) then
					FindFirstChild(v, AlwaysNameESP):Destroy(true);
				end;
			end);
		end;
	end;



	-- System

	ForceBreak = false;

	if not GG["DisableSystem"] then


		FirstMobTarget = nil;
		FirstMobTargetPos = nil;

		AnyProcessOnline = false;
		AnyStatsC = false;
		AnyBringMobC = false;

		AnySea1 = false;
		AnySea1W = false;
		AnySea2 = false;
		AnySea2W = false;
		AnySea3 = false;
		AnySea3W = false;

		IsEliteHunterSpawn = false;
		EliteKilled = 0;

		FPS = 0;

		SystemConnection1 = H.Stepped:Connect(function()
			if ForceBreak or GG["DisableSystem"] then
				ForceBreak = true;
				SystemConnection1:Disconnect();
				SystemConnection1 = nil;
			end;
			selc = selff.Character or nil;
			AnyProcessOnline = ProcessAutoChest;
			Backpack = selff.Backpack;
			AnyStats = Configs.Stats.Melee or Configs.Stats.Defense or Configs.Stats.Sword or Configs.Stats.Gun or Configs.Stats.BloxFruit;
			AnySea1 = Configs.Farming.Sea1.AutoSaberQuest or Configs.Farming.Sea1.AutoSaber or Configs.Farming.Sea1.AutoSaw;
			AnySea2 = Configs.Farming.Sea2.AutoFactory or Configs.Farming.Sea2.AutoEctoplasm or Configs.Farming.Sea2.AutoBartilo;
			AnySea3 = Configs.Farming.Sea3.AutoKillDoughKing or Configs.Farming.Sea3.AutoElitehunter or Configs.Farming.Sea3.AutoFarmBone or Configs.Farming.Sea3.AutoCakePrince or Configs.Farming.Sea3.AutoBuddySword;
			AnySea1W = Configs.Farming.Sea1.AutoSaber;
			AnySea2W = Configs.Farming.Sea2.AutoFactory or Configs.Farming.Sea2.AutoBartilo;
			AnySea3W = Configs.Farming.Sea3.AutoKillDoughKing or Configs.Farming.Sea3.AutoElitehunter or Configs.Farming.Sea3.AutoCakePrince or Configs.Farming.Sea3.AutoBuddySword;
			NeedAttacking = (Configs.Farming.Level.AutoFarm) or (Configs.Farming.Sea1.AutoSaber or Configs.Farming.Sea1.AutoSaberQuest) or (Configs.Farming.Sea2.AutoFactory) or (Configs.Farming.Sea1.AutoSaw) or (Configs.Farming.Sea2.AutoEctoplasm) or (Configs.Farming.Sea2.AutoBartilo) or (Configs.Farming.Sea3.AutoKillDoughKing) or (Configs.Farming.Sea3.AutoElitehunter) or (Configs.Farming.Sea3.AutoFarmBone) or (Configs.Farming.Sea3.AutoCakePrince) or (Configs.Farming.Sea3.AutoBuddySword) or (Configs.System.Selected_Mob.Auto or Configs.System.Selected_Boss.Auto);
			if AnyStats and not AnyStatsC and Data.Points.Value > 0 then
				AnyStatsC = true;
				if Configs.Stats.Melee then
					CommF:InvokeServer("AddPoint", "Melee", Configs.Stats.Points);
				end;
				if Configs.Stats.Defense then
					CommF:InvokeServer("AddPoint", "Defense", Configs.Stats.Points);
				end;
				if Configs.Stats.Sword then
					CommF:InvokeServer("AddPoint", "Sword", Configs.Stats.Points);
				end;
				if Configs.Stats.Gun then
					CommF:InvokeServer("AddPoint", "Gun", Configs.Stats.Points);
				end;
				if Configs.Stats.BloxFruit then
					CommF:InvokeServer("AddPoint", "Demon Fruit", Configs.Stats.Points);
				end;
				twait(0.5); AnyStatsC = false;
			end;
			Enem = Enemy:GetChildren();
			Chars = W.Characters:GetChildren();
			if Configs.Settings.AutoFarm.BringMob and not AnyBringMobC and #Enem > 0 then
				AnyBringMobC = true;
				if Configs.Settings.AutoFarm.BringMobType == "Target" then
					if FirstMobTarget == nil or (FirstMobTarget ~= nil and (not FirstMobTarget.Parent or not FindFirstChild(FirstMobTarget, "HumanoidRootPart"))) or (FirstMobTarget ~= nil and FindFirstChild(FirstMobTarget, "Humanoid") and FirstMobTarget.Humanoid.Health == 0) then
						pcal(function()
							for _, v in pir(Enem) do
								if v and IsA(v, "Model") and v.Name == TargetMobName and FindFirstChild(v, "HumanoidRootPart") and dist(v.HumanoidRootPart.Position) <= Configs.Settings.AutoFarm.BringMobDistance then
									FirstMobTarget = v;
									FirstMobTargetPos = GetPivot(v);
								end;
							end;
						end);
					else
						pcal(function()
							for _, v in pir(Enem) do
								if v and IsA(v, "Model") and v.Name == TargetMobName and FindFirstChild(v, "HumanoidRootPart") and dist(v.HumanoidRootPart.Position) <= Configs.Settings.AutoFarm.BringMobDistance and (FirstMobTargetPos.Position - v.HumanoidRootPart.Position).Magnitude <= Configs.Settings.AutoFarm.BringMobDistance then
									v.HumanoidRootPart.CFrame = FirstMobTargetPos;
									SetHum(v);
								end;
							end;
						end);
					end;
				elseif Configs.Settings.AutoFarm.BringMobType == "Any" then
					if FirstMobTarget == nil or (FirstMobTarget ~= nil and (not FirstMobTarget.Parent or not FindFirstChild(FirstMobTarget, "HumanoidRootPart"))) or (FirstMobTarget ~= nil and FindFirstChild(FirstMobTarget, "Humanoid") and FirstMobTarget.Humanoid.Health == 0) then
						pcal(function()
							for _, v in pir(Enem) do
								if v and IsA(v, "Model") and FindFirstChild(v, "HumanoidRootPart") and dist(v.HumanoidRootPart.Position) <= Configs.Settings.AutoFarm.BringMobDistance then
									FirstMobTarget = v;
									FirstMobTargetPos = GetPivot(v);
								end;
							end;
						end);
					else
						pcal(function()
							for _, v in pir(Enem) do
								if v and IsA(v, "Model") and FindFirstChild(v, "HumanoidRootPart") and dist(v.HumanoidRootPart.Position) <= Configs.Settings.AutoFarm.BringMobDistance and (FirstMobTargetPos.Position - v.HumanoidRootPart.Position).Magnitude <= Configs.Settings.AutoFarm.BringMobDistance then
									v.HumanoidRootPart.CFrame = FirstMobTargetPos;
									SetHum(v);
								end;
							end;
						end);
					end;
				end;
				twait(1);
				AnyBringMobC = false;
			end;
			if Configs.Players.Client.AutoBusoHaki and not FindFirstChild(selc, "HasBuso") then
				CommF:InvokeServer("Buso")
			end;
			if Configs.Players.Client.InfiniteEnergy then
				InfStamina();
			end;
			if NeedAttacking and FindFirstChild(selc, "Humanoid") and selc.Humanoid.Health > 1 and not IgnoreE then
				EquipWeapon(Configs.Farming.Weapon);
			end;
			if Configs.Players.Client.Noclip then
				for _, child in pir(GetDescendants(selc)) do
					if IsA(child, "BasePart") then
						child.CanCollide = false;
					end;
				end;
			end;
			PhysicsFPST.Set("Physics FPS: " .. tos(W:GetRealPhysicsFPS()));
			ClientFPST.Set("Client FPS: ".. FPS);
			PingT.Set("Ping: ".. (game:GetService("Stats")).Network.ServerStatsItem["Data Ping"]:GetValueString());
			ExecutorT.Set("Executor: " .. Iden);
			UNCPreparedAsT.Set("UNC Prepared: " .. Iden);
			IsselcT.Set( "Is Character: " .. tos(selc));
			MuchBackpackT.Set("Backpack: " .. tos(Backpack and #GetChildren(Backpack)));
			AnyStatsT.Set("AnyStats: " .. tos(AnyStats));
			AnySea1T.Set("AnySea1: " .. tos(AnySea1));
			AnySea2T.Set("AnySea2: " .. tos(AnySea2));
			AnySea3T.Set("AnySea3: " .. tos(AnySea3));
			AnySea1WT.Set("AnySea1WT: " .. tos(AnySea1W));
			AnySea2WT.Set("AnySea2WT: " .. tos(AnySea2W));
			AnySea3WT.Set("AnySea3WT: " .. tos(AnySea3W));
			NeedAttackingT.Set("NeedAttacking: " .. tos(NeedAttacking));
			MuchEnemT.Set("Enem: " .. tos(Enem and #Enem));
			CharsT.Set("Chars: " .. tos(Chars and #Chars));
			IsEliteHunterSpawn = FindFirstChild(R, "Diablo") or FindFirstChild(R, "Deandre") or FindFirstChild(R, "Urban") or FindFirstChild(Enemy, "Diablo") or FindFirstChild(Enemy, "Deandre") or FindFirstChild(Enemy, "Urban") or false;
			EliteHunterSpawnT.Set("Elite Spawn: " .. tos(IsEliteHunterSpawn));
			EliteKilledT.Set("Elite Killed: " .. tos(EliteKilled));
			if AnyESP then

			end;
			ActiveTimeT.Set("Active Time: " .. tos(GG.ActiveTime) .. "s");
		end);

		if not FindFirstChild(W, "selcRootX") then
			UFPart = Instancen("Part");
			UFPart.Size = Vec3(2, 0.2, 1.5);
			UFPart.Material = Enum.Material.Grass;
			UFPart.Anchored = true;
			UFPart.Transparency = 1;
			UFPart.Parent = W;
			UFPart.Name = "selcRootX";
		else
			UFPart = FindFirstChild(W, "selcRootX");
		end;
		SystemConnection2 = H.RenderStepped:Connect(function()
			if ForceBreak or GG["DisableSystem"] then
				ForceBreak = true;
				SystemConnection2:Disconnect();
				SystemConnection2 = nil;
			end;
			if not UFPart then
				UFPart = FindFirstChild(W, "selcRootX");
			end;
			local HumRSelf = selc and FindFirstChild(selc, "HumanoidRootPart");
			if UFPart and HumRSelf and Configs.Players.Client.Float then
				UFPart.CFrame = HumRSelf.CFrame * CF(0, -3.1, 0);
			else
				UFPart.CFrame = CF(0, -10000, 0);
			end;
			if Configs.Players.Client.WalkSpeedToggle then
				SetAttribute(selc, "SpeedMultiplier", Configs.Players.Client.WalkSpeed);
				selc.Humanoid.WalkSpeed = 36 * Configs.Players.Client.WalkSpeed;
			end;
			if Configs.Players.Client.JumpPowerToggle then
				selc.Humanoid.JumpPower = Configs.Players.Client.JumpPower;
			end;
			if Configs.Players.Client.WalkOnWater then
				W.Map["WaterBase-Plane"].Size = Vector3.new(1000, 112, 1000);
			else
				W.Map["WaterBase-Plane"].Size = Vector3.new(1000, 80, 1000);
			end;
		end);

		framesFPS = 0;
		lastTimeFPS = tick();
		FPS = 0;

		SystemConnection3 = H.RenderStepped:Connect(function()
			framesFPS = framesFPS + 1;
			local currentTime = tick();
			
			if currentTime - lastTimeFPS >= 1 then
				FPS = framesFPS;
				framesFPS = 0;
				lastTimeFPS = currentTime;
			end
		end);

		tspawn(function()
			while twait(3) do
				if not Configs.Settings.AutoFarm.Stackable then
					Notification.new({
						Description = "Failed to Stack (Turn on Stackable in Settings)";
						Title = "Neuron X [Error]";
						Duration = 3;
					});
				end;
			end;
		end);

		tspawn(function()
			while twait() do --Active0SecondScriptSystem
				if ForceBreak or GG["DisableSystem"] then
					ForceBreak = true;
					break;
				end;
				pcal(function()
					if Configs.Raid.Manual.KillAura then
						for _, v in pir(Enem) do
							if v then
								v.Humanoid.Health = 0;
							end;
						end;
					end;
				end);
				if Configs.System.Selected_Boss.Auto and Configs.System.Selected_Boss.Obj then
					pcal(function()
						NewValueBossREpFW = FindFirstChild(Enemy, Configs.System.Selected_Boss.Obj) or FindFirstChild(R, Configs.System.Selected_Boss.Obj);
						if NewValueBossREpFW then
							if NewValueBossREpFW.Parent == Enemy then
								if FindFirstChild(NewValueBossREpFW, "Humanoid") and FindFirstChild(NewValueBossREpFW, "HumanoidRootPart") and NewValueBossREpFW.Humanoid.Health > 0 then
									sethiddenproperty(selff, "SimulationRadius", math.huge);
									repeat twait(); TargetMobName = NewValueBossREpFW.Name;
										Tp(NewValueBossREpFW.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
										SetHum(NewValueBossREpFW); Hitc();
									until not NewValueBossREpFW or not Configs.System.Selected_Boss.Auto or NewValueBossREpFW.Humanoid.Health <= 0
								end;
							elseif NewValueBossREpFW.Parent == R then
								if Configs.Teleport.TP_Bypass and NewValueBossREpFW then
									TpBypass(GetPivot(NewValueBossREpFW));
									twait(0.7);
								end;
								Tp(NewValueBossREpFW.HumanoidRootPart.CFrame * CF(0,35,0), Configs.Settings.AutoChest.TP_Speed);
							end;
						end;
					end);
				end;
				if (FindFirstChild(Enemy, "Shark") and Configs.SeaEvents.Target.Shark) or (FindFirstChild(Enemy, "Terrorshark") and Configs.SeaEvents.Target.Terrorshark) or (FindFirstChild(Enemy, "Piranha") and Configs.SeaEvents.Target.Piranha) or (FindFirstChild(Enemy, "Fish Crew Member") and Configs.SeaEvents.Target.FishCrewMember) then
					Configs.System.BREAKALLTHISSHITHAHAHAHAHA = true;
					twait(0.78);
					Configs.System.BREAKALLTHISSHITHAHAHAHAHA = false;
				end;
			end;
		end);
		tspawn(function()
			while twait() do --2ndActive0SecondScriptSystem
				if Configs.System.Astaootslai then
					pcal(function()
						GettingAllSeaEvents = FindFirstChild(Enemy, "Shark") or FindFirstChild(Enemy, "Terrorshark") or FindFirstChild(Enemy, "Piranha") or FindFirstChild(Enemy, "Fish Crew Member");
						if not GettingAllSeaEvents then
							if not FindBoat() then
								if dist(Vec3(-16927.451171875, 9.0863618850708, 433.8642883300781)) > 1000 then
									if Configs.Teleport.TP_Bypass then
										TpBypass(CF(-16927.451171875, 9.0863618850708, 433.8642883300781));
										twait(0.7);
									end;
									Tp(CF(-16927.451171875, 9.0863618850708, 433.8642883300781), Configs.Settings.AutoFarm.TP_Speed or 4);
									if dist(Vec3(-16927.451171875, 9.0863618850708, 433.8642883300781)) <= 10 then
										CommF:InvokeServer("BuyBoat", "PirateBrigade")
									end;
								else
									Tp(CF(-16927.451171875, 9.0863618850708, 433.8642883300781), Configs.Settings.AutoFarm.TP_Speed or 4);
									if dist(Vec3(-16927.451171875, 9.0863618850708, 433.8642883300781)) <= 10 then
										CommF:InvokeServer("BuyBoat", "PirateBrigade")
									end;
								end;
							elseif FindBoat() then
								CurrentBoatReturn = FindBoat();
								if CurrentBoatReturn and FindFirstChild(CurrentBoatReturn, "VehicleSeat") and selff.Character and FindFirstChild(selff.Character, "Humanoid") and not selff.Character.Humanoid.Sit then
									Tp(CurrentBoatReturn.VehicleSeat.CFrame, Configs.Settings.AutoFarm.TP_Speed or 4);
								else
									repeat twait();
										whywechoosencs = math.random(1, 3)
										if whywechoosencs == 1 then
											tpwithseat(CF(-33163.1875, 10.964323997497559, -324.4842224121094), Configs.SeaEvents.TP_Speed);
										elseif whywechoosencs == 2 then
											tpwithseat(CF(-37952.49609375, 10.96342945098877, -1324.12109375), Configs.SeaEvents.TP_Speed);
										elseif whywechoosencs == 3 then
											tpwithseat(CF(-33163.1875, 10.964323997497559, -324.4842224121094), Configs.SeaEvents.TP_Speed);
										end;
									until Configs.System.BREAKALLTHISSHITHAHAHAHAHA or not Configs.System.Astaootslai or (FindFirstChild(Enemy, "Shark") and Configs.SeaEvents.Target.Shark) or (FindFirstChild(Enemy, "Terrorshark") and Configs.SeaEvents.Target.Terrorshark) or (FindFirstChild(Enemy, "Piranha") and Configs.SeaEvents.Target.Piranha) or (FindFirstChild(Enemy, "Fish Crew Member") and Configs.SeaEvents.Target.FishCrewMember);
								end;
							end;
						end;
					end);
				end; twait();
				pcal(function()
					if (FindFirstChild(Enemy, "Shark") and Configs.SeaEvents.Target.Shark) then
						selff.Character.Humanoid.Sit = false; twait(0.3);
						repeat twait(); until FindFirstChild(Enemy.Shark, "Humanoid") and FindFirstChild(Enemy.Shark, "HumanoidRootPart")
						SetKillTarget("Shark", Configs.SeaEvents.Target.Shark);
					end; twait();
					if (FindFirstChild(Enemy, "Terrorshark") and Configs.SeaEvents.Target.Terrorshark) then
						selff.Character.Humanoid.Sit = false; twait(0.3);
						repeat twait(); until FindFirstChild(Enemy.Terrorshark, "Humanoid") and FindFirstChild(Enemy.Terrorshark, "HumanoidRootPart")
						SetKillTarget("Terrorshark", Configs.SeaEvents.Target.Terrorshark);
					end; twait();
					if (FindFirstChild(Enemy, "Piranha") and Configs.SeaEvents.Target.Piranha) then
						selff.Character.Humanoid.Sit = false; twait(0.3);
						repeat twait(); until FindFirstChild(Enemy.Piranha, "Humanoid") and FindFirstChild(Enemy.Piranha, "HumanoidRootPart")
						SetKillTarget("Piranha", Configs.SeaEvents.Target.Piranha);
					end; twait();
					if (FindFirstChild(Enemy, "Fish Crew Member") and Configs.SeaEvents.Target.FishCrewMember) then
						selff.Character.Humanoid.Sit = false; twait(0.3);
						SetKillTarget("Fish Crew Member", Configs.SeaEvents.Target.FishCrewMember);
					end;
				end); twait();
			end;
		end);

		tspawn(function()
			while twait(1) do --Active1SecondScriptSystem
				if ForceBreak or GG["DisableSystem"] then
					ForceBreak = true;
					break;
				end;
				pcal(function()
					GG.ActiveTime = GG.ActiveTime + 1;
				end);
				pcal(function()
					EliteKilled = CommF:InvokeServer("EliteHunter", "Progress");
				end);
			end;
		end);
		AnyESP = true; -- testing
		tspawn(function()
			while twait(2) do --Active2SecondScriptSystem
				if ForceBreak or GG["DisableSystem"] then
					ForceBreak = true;
					break;
				end;
				if AnyESP then
					UpdateP();
					UpdateESPOver();
					UpdateRLFr();
					UpdateG();
					UpdateAFD();
					UpdateIsland();
				end;
				if Configs.Farming.Sea3.AutoSpawnCakePrince then
					if WorldCheck["Third Sea"] then
						CommF:InvokeServer("CakePrinceSpawner", true);
					end;
				end;
			end;
		end);

		StackableError = function(Name, Text)
			print(Name)
		end;

		AnyMelee = true; -- Testing purpose

		CurrentFocusMelee = "nil"

		tspawn(function()
			while twait() do
				if ForceBreak or GG["DisableSystem"] then
					ForceBreak = true;
					break;
				end;
				--pcal(function()
					Level =	Data.Level.Value;
					if Configs.Settings.AutoFarm.Stackable then
						if not AnyProcessOnline then
							if Configs.System.AutoChest then
								AutoChest(false);
							end;
						end;
					end;
					if AnySea1 then
						if AnySea1W then
							if Configs.Farming.Sea1.AutoSaber and not getInventory("Saber", "Check") then
								if FindFirstChild(Enemy, "Saber Expert") then
									pcal(function()
										for i,v in pir(Enem) do
											if v.Name == "Saber Expert" then
												if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
													sethiddenproperty(selff,"SimulationRadius",math.huge);
													repeat twait(); TargetMobName = v.Name;
														Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
														SetHum(v); Hitc();
													until not v or not Configs.Farming.Sea1.AutoSaber or v.Humanoid.Health <= 0
												end;
											end;
										end;
									end);
								else
									pcal(function()
										if FindFirstChild(R, "Saber Expert") then
											if Configs.Teleport.TP_Bypass and R["Saber Expert"] then
												TpBypass(GetPivot(R["Saber Expert"]));
											end; twait(0.7)
											Tp(FindFirstChild(R, "Saber Expert").HumanoidRootPart.CFrame * CF(0,35,0), Configs.Settings.AutoChest.TP_Speed);
											wait(1.3);
											pcal(function()
												for i,v in pir(Enem) do
													if v.Name == "Saber Expert" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															sethiddenproperty(selff,"SimulationRadius",math.huge);
															repeat task.wait(); TargetMobName = v.Name;
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															until not v or not Configs.Farming.Sea1.AutoSaber or v.Humanoid.Health <= 0
														end;
													end;
												end;
											end);
										else
											if Configs.Farming.Sea1.Hop then
												task.wait(3);
												if (not FindFirstChild(R, "Saber Expert") or not FindFirstChild(Enemy, "Saber Expert")) and Configs.Farming.Sea1.Hop and not Configs.Settings.AutoFarm.Stackable then
													Hop();
												end;
											end;
										end;
									end);
								end;
							end;
							if Configs.Farming.Sea1.AutoSaw and not getInventory("Shark Saw", "Check") then
								repeat twait();
									if FindFirstChild(Enemy, "The Saw") then
										if Configs.Teleport.TP_Bypass and R["The Saw"] then
											TpBypass(GetPivot(R["The Saw"]));
										end; twait(0.7)
										pcal(function()
											for i,v in pir(Enem) do
												if v.Name == "The Saw" then
													if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
														sethiddenproperty(selff,"SimulationRadius",math.huge);
														repeat task.wait(); TargetMobName = v.Name;
															Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
															SetHum(v); Hitc();
														until not v or not Configs.Farming.Sea1.AutoSaw or v.Humanoid.Health <= 0
													end;
												end;
											end;
										end);
									else
										pcal(function()
											if FindFirstChild(R, "The Saw") then
												if Configs.Teleport.TP_Bypass and R["The Saw"] then
													TpBypass(GetPivot(R["The Saw"]));
												end; twait(0.7)
												Tp(FindFirstChild(R, "The Saw").HumanoidRootPart.CFrame * CF(0,35,0), Configs.Settings.AutoChest.TP_Speed);
												twait(1.3);
												pcal(function()
													for i,v in pir(Enem) do
														if v.Name == "The Saw" then
															if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
																sethiddenproperty(selff,"SimulationRadius",math.huge);
																repeat task.wait(); TargetMobName = v.Name;
																	Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																	SetHum(v); Hitc();
																until not v or not Configs.Farming.Sea1.AutoSaw or v.Humanoid.Health <= 0
															end;
														end;
													end;
												end);
											else
												if Configs.Farming.Sea1.Hop then
													task.wait(3);
													if (not FindFirstChild(R, "The Saw") or not FindFirstChild(Enemy, "The Saw")) and Configs.Farming.Sea1.HopSaw then
														Hop();
													end;
												end;
											end;
										end);
									end;
								until not FindFirstChild(R, "The Saw") or not FindFirstChild(Enemy, "The Saw")
							end;
						end;
						if Configs.Farming.Sea1.AutoSaberQuest and FindFirstChild(W, "Map") and FindFirstChild(W.Map, "Jungle") and FindFirstChild(W.Map.Jungle, "Final") then
							if W.Map.Jungle.Final:FindFirstChild("Part").Transparency == 0 and Level >= 200 then
								CommonSenz, CommonError = pcall(function()
									IgnoreE = true;
									if not (W and FindFirstChild(W.Map, "Desert") and FindFirstChild(W.Map.Desert, "Burn") and FindFirstChild(W.Map.Desert.Burn, "Part").Transparency == 1) then
										repeat
											repeat
												Tp(CF(-1421.6824951171875, 48.252071380615234, 21.318946838378906), Configs.Settings.AutoFarm.TP_Speed); twait(2);
											until dist(Vec3(-1421.6824951171875, 48.252071380615234, 21.318946838378906)) <= 10
											Tp(CF(-1181.1640625, 22.34051513671875, 188.13186645507812), Configs.Settings.AutoFarm.TP_Speed); twait(0.5);
											Tp(CF(-1648.1024169921875, 23.252126693725586, 438.4625549316406), Configs.Settings.AutoFarm.TP_Speed); twait(0.5);
											Tp(CF(-1153.3184814453125, 2.464047908782959, -701.0916748046875), Configs.Settings.AutoFarm.TP_Speed); twait(0.5);
											Tp(CF(-1325.6697998046875, 34.64987564086914, -463.0443420410156), Configs.Settings.AutoFarm.TP_Speed); twait(0.7);
											Tp(CF(-1610.5228271484375, 12.052069664001465, 162.6676025390625), Configs.Settings.AutoFarm.TP_Speed); twait(0.6);
											Tp2(CF(-1610.00757, 11.5049858, 164.001587)); wait(0.5);
										until FindFirstChild(Backpack, "Torch") or FindFirstChild(selff.Character, "Torch") or not Configs.Farming.Sea1.AutoSaberQuest or (W and FindFirstChild(W.Map, "Desert") and FindFirstChild(W.Map.Desert, "Burn") and FindFirstChild(W.Map.Desert.Burn, "Part").Transparency == 1)
									end;
									if CommF:InvokeServer("ProQuestProgress", "SickMan") ~= 0 then
										repeat
											EquipWeapon("Torch"); twait(0.5);
											Tp(CF(1115.79688, 4.92147732, 4350.17334), Configs.Settings.AutoFarm.TP_Speed); twait(1.5);
											Tp(CF(1113.51929, 5.50669432, 4365.20752), Configs.Settings.AutoFarm.TP_Speed); twait(0.5);
											Tp2(CF(1114.61475, 5.04679728, 4350.22803, -0.648466587, -0.00000000128799094, 0.761243105, -0.000000000570652914, 1, 0.00000000120584542, -0.761243105, 0.000000000347544882, -0.648466587)); wait(0.3);
										until FindFirstChild(Backpack, "Cup") or FindFirstChild(selff.Character, "Cup") or not Configs.Farming.Sea1.AutoSaberQuest
										EquipWeapon("Cup"); twait(0.8);
										Tp(CF(1392.83411, 37.3479347, -1323.19702), Configs.Settings.AutoFarm.TP_Speed); twait(5); CommF:InvokeServer("ProQuestProgress", "FillCup", selff.Character.Cup);
										Tp(CF(1458.23046875, 88.25215911865234, -1389.040283203125), Configs.Settings.AutoFarm.TP_Speed); twait(1.2);
										CommF:InvokeServer("ProQuestProgress", "SickMan"); twait(0.6);
									end;
									Tp(CF(-908.8209838867188, 13.752044677734375, 4078.2666015625), Configs.Settings.AutoFarm.TP_Speed); twait(1.2);
									CommF:InvokeServer("ProQuestProgress", "RichSon"); twait(0.5);
									Tp(CF(-2850.20068, 7.39224768, 5354.99268), Configs.Settings.AutoFarm.TP_Speed); twait(1);
									repeat twait(); until FindFirstChild(Enemy, "Mob Leader") or not Configs.Farming.Sea1.AutoSaberQuest
									if not Configs.Farming.Sea1.AutoSaberQuest then
										return false;
									end;
									if FindFirstChild(Enemy, "Mob Leader") then
										for i,v in pairs(Enemy:GetChildren()) do
											if v.Name == "Mob Leader" then
												if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
													repeat twait(); TargetMobName = v.Name;
														Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
														SetHum(v); sethiddenproperty(selff, "SimulationRadius", math.huge); Hitc();
													until not v or not Configs.Farming.Sea1.AutoSaberQuest or v.Humanoid.Health <= 0
												end;
											end;
										end;
									end; twait(0.6);
									Tp(CF(-908.8209838867188, 13.752044677734375, 4078.2666015625), Configs.Settings.AutoFarm.TP_Speed); twait(1.2);
									CommF:InvokeServer("ProQuestProgress", "RichSon"); twait(1.1);
									CommF:InvokeServer("ProQuestProgress"); twait(0.5); EquipWeapon("Relic");
									Tp(CF(-1405.82397, 29.8520069, 5.05986547), Configs.Settings.AutoFarm.TP_Speed); twait(0.3);
									selff.Character.HumanoidRootPart.CFrame = CF(-1405.82397, 29.8520069, 5.05986547, 0.759286761, 4.36840342e-09, 0.65075618, 9.78191306e-09, 1, -1.81261139e-08, -0.65075618, 2.01285584e-08, 0.759286761);
								end); twait();
								if not CommonSenz then
									warn(CommonError);
									if Lib and Lib.SendConsole then
										Lib:SendConsole(tos(CommonError), Color3.fromRGB(255, 0, 0))
									end;
								end;
								IgnoreE = false;
							end;
						end;
					end;
					if AnySea2 then
						if AnySea2W then
							if Configs.Farming.Sea2.AutoFactory then
								if FindFirstChild(Enemy, "Core") then
									for i,v in pir(Enem) do
										if v.Name == "Core" then
											if FindFirstChild(v, "Humanoid") and v.Humanoid.Health > 0 then
												sethiddenproperty(selff,"SimulationRadius",math.huge);
												repeat twait();
													Tp(CF(448.46756, 199.356781, -441.389252), Configs.Settings.AutoFarm.TP_Speed);
													Hitc();
												until not v or not Configs.Farming.Sea2.AutoFactory or (FindFirstChild(v, "Humanoid") and v.Humanoid.Health <= 0)
											end;
										end;
									end;
								elseif FindFirstChild(R, "Core") then
									if Configs.Teleport.TP_Bypass then
										TpBypass(CF(448.46756, 199.356781, -441.389252));
									end; twait(0.7);
									Tp(CF(448.46756, 199.356781, -441.389252), 2); twait(0.3);
									for i,v in pir(Enem) do
										if v.Name == "Core" then
											if FindFirstChild(v, "Humanoid") and v.Humanoid.Health > 0 then
												sethiddenproperty(selff,"SimulationRadius",math.huge);
												repeat twait();
													Tp(CF(448.46756, 199.356781, -441.389252), Configs.Settings.AutoFarm.TP_Speed);
													Hitc();
												until not v or not Configs.Farming.Sea2.AutoFactory or (FindFirstChild(v, "Humanoid") and v.Humanoid.Health <= 0)
											end;
										end;
									end;
								end;
							end;
							if Configs.Farming.Sea2.AutoBartilo then
								if selff.Data.Level.Value >= 800 then
									if CommF:InvokeServer("BartiloQuestProgress", "Bartilo") == 0 then
										if strfind(PSG.Main.Quest.Container.QuestTitle.Title.Text, "Swan Pirates") and strfind(PSG.Main.Quest.Container.QuestTitle.Title.Text, "50") and PSG.Main.Quest.Visible then
											warn(dist(Vec3(932.624451, 156.106079, 1180.27466)))
											repeat twait();
												if FindFirstChild(Enemy, "Swan Pirate") then
													TargetMobName = "Swan Pirate";
													pcal(function()
														for i,v in pir(Enemy:GetChildren()) do
															if v.Name == "Swan Pirate" then
																if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
																	sethiddenproperty(selff,"SimulationRadius",math.huge);
																	TargetMobName = v.Name;
																	repeat twait();
																		Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																		SetHum(v); Hitc();
																	until not v or v.Humanoid.Health <= 0 or not PSG.Main.Quest.Visible or not Configs.Farming.Sea2.AutoBartilo or CommF:InvokeServer("BartiloQuestProgress", "Bartilo") ~= 0
																end;
															end;
														end;
													end);
												else
													repeat twait();
														Tp(CF(932.624451, 156.106079, 1180.27466), Configs.Settings.AutoFarm.TP_Speed);
													until dist(Vec3(932.624451, 156.106079, 1180.27466)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
												end;
											until not PSG.Main.Quest.Visible or not Configs.Farming.Sea2.AutoBartilo or PData.DidBartilo or CommF:InvokeServer("BartiloQuestProgress", "Bartilo") ~= 0
										else
											repeat twait();
												Tp(CF(-456.28952, 73.0200958, 299.895966), Configs.Settings.AutoFarm.TP_Speed);
											until dist(Vec3(-456.28952, 73.0200958, 299.895966)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
											twait(1.1);
											CommF:InvokeServer("StartQuest", "BartiloQuest", 1);
											repeat twait();
												if FindFirstChild(Enemy, "Swan Pirate") then
													TargetMobName = "Swan Pirate";
													pcal(function()
														for i,v in pir(Enemy:GetChildren()) do
															if v.Name == "Swan Pirate" then
																if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
																	sethiddenproperty(selff,"SimulationRadius",math.huge);
																	TargetMobName = v.Name;
																	repeat twait();
																		Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																		SetHum(v); Hitc();
																	until not v or v.Humanoid.Health <= 0 or not PSG.Main.Quest.Visible or not Configs.Farming.Sea2.AutoBartilo or CommF:InvokeServer("BartiloQuestProgress", "Bartilo") ~= 0
																end;
															end;
														end;
													end);
												else
													repeat twait();
														Tp(CF(932.624451, 156.106079, 1180.27466), Configs.Settings.AutoFarm.TP_Speed);
													until dist(Vec3(932.624451, 156.106079, 1180.27466)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
												end;
											until not PSG.Main.Quest.Visible or not Configs.Farming.Sea2.AutoBartilo or PData.DidBartilo or CommF:InvokeServer("BartiloQuestProgress", "Bartilo") ~= 0
										end;
									elseif CommF:InvokeServer("BartiloQuestProgress", "Bartilo") == 1 then
										if FindFirstChild(Enemy, "Jeremy") then
											pcal(function()
												for i,v in pir(Enemy:GetChildren()) do
													if v.Name == "Jeremy" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															sethiddenproperty(selff,"SimulationRadius",math.huge);
															TargetMobName = v.Name;
															repeat twait();
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															until not v or v.Humanoid.Health <= 0 or not Configs.Farming.Sea2.AutoBartilo or CommF:InvokeServer("BartiloQuestProgress", "Bartilo") ~= 1
														end;
													end;
												end;
											end);
										elseif FindFirstChild(R, "Jeremy") then
											repeat twait();
												Tp(CF(2335.0498, 449.056244, 731.760559), Configs.Settings.AutoFarm.TP_Speed);
											until dist(Vec3(2335.0498, 449.056244, 731.760559)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
											twait(0.5)
											pcal(function()
												for i,v in pir(Enemy:GetChildren()) do
													if v.Name == "Jeremy" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															sethiddenproperty(selff,"SimulationRadius",math.huge);
															TargetMobName = v.Name;
															repeat twait();
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															until not v or v.Humanoid.Health <= 0 or not Configs.Farming.Sea2.AutoBartilo or CommF:InvokeServer("BartiloQuestProgress", "Bartilo") ~= 1
														end;
													end;
												end;
											end);
										end;
									elseif CommF:InvokeServer("BartiloQuestProgress", "Bartilo") == 2 then
										repeat
											Tp(CF(-1850.49329, 13.1789551, 1750.89685), Configs.Settings.AutoFarm.TP_Speed);
											twait();
										until dist(Vec3(-1850.49329, 13.1789551, 1750.89685)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
										twait(1);
										repeat
											Tp(CF(-1858.87305, 19.3777466, 1712.01807), Configs.Settings.AutoFarm.TP_Speed);
											twait();
										until dist(Vec3(-1858.87305, 19.3777466, 1712.01807)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
										twait(1);
										repeat
											Tp(CF(-1803.94324, 16.5789185, 1750.89685), Configs.Settings.AutoFarm.TP_Speed);
											twait();
										until dist(Vec3(-1803.94324, 16.5789185, 1750.89685)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
										twait(1);
										repeat
											Tp(CF(-1858.55835, 16.8604317, 1724.79541), Configs.Settings.AutoFarm.TP_Speed);
											twait();
										until dist(Vec3(-1858.55835, 16.8604317, 1724.79541)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
										twait(1);
										repeat
											Tp(CF(-1869.54224, 15.987854, 1681.00659), Configs.Settings.AutoFarm.TP_Speed);
											twait();
										until dist(Vec3(-1869.54224, 15.987854, 1681.00659)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
										twait(1);
										repeat
											Tp(CF(-1800.0979, 16.4978027, 1684.52368), Configs.Settings.AutoFarm.TP_Speed);
											twait();
										until dist(Vec3(-1800.0979, 16.4978027, 1684.52368)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
										twait(1);
										repeat
											Tp(CF(-1819.26343, 14.795166, 1717.90625), Configs.Settings.AutoFarm.TP_Speed);
											twait();
										until dist(Vec3(-1819.26343, 14.795166, 1717.90625)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
										twait(1);
										repeat
											Tp(CF(-1813.51843, 14.8604736, 1724.79541), Configs.Settings.AutoFarm.TP_Speed);
											twait();
										until dist(Vec3(-1813.51843, 14.8604736, 1724.79541)) <= 10 or not Configs.Farming.Sea2.AutoBartilo;
									else
										PData.DidBartilo = true;
									end;
								end;
							end;
						end;
						if Configs.Farming.Sea2.AutoEctoplasm then
							EctoplasmMobList = {"Ship Deckhand", "Ship Engineer", "Ship Steward", "Ship Officer"};
							if dist(Vec3(911.35827636719, 125.95812988281, 33159.5390625)) >= 4700 then
								CommF:InvokeServer("requestEntrance", Vec3(923.21252441406, 126.9760055542, 32852.83203125));
							end; twait(1);
							for _, v in pir(Enemy:GetChildren()) do
								if Configs.Farming.Sea2.AutoEctoplasm and v and table.find(EctoplasmMobList, v.Name) and FindFirstChild(v, "Humanoid") and v.Humanoid.Health > 0 then
									sethiddenproperty(selff, "SimulationRadius", math.huge);
									repeat twait(); TargetMobName = v.Name;
										Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), 3);
										SetHum(v); Hitc();
									until not v or not Configs.Farming.Sea2.AutoEctoplasm or v.Humanoid.Health <= 0
								end;
							end;
						end;
					end;
					if AnySea3 then
						if AnySea3W then
							if Configs.Farming.Sea3.AutoBuddySword then
								if FindFirstChild(Enemy, "Cake Queen") then
									pcal(function()
										for i,v in pir(Enem) do
											if v.Name == "Cake Queen" then
												if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
													sethiddenproperty(selff,"SimulationRadius",math.huge);
													repeat twait(); TargetMobName = v.Name;
														Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
														SetHum(v); Hitc();
													until not v or not Configs.Farming.Sea3.AutoBuddySword or v.Humanoid.Health <= 0
												end;
											end;
										end;
									end);
								else
									pcal(function()
										if FindFirstChild(R, "Cake Queen") then
											if Configs.Teleport.TP_Bypass and R["Cake Queen"] then
												TpBypass(GetPivot(R["Cake Queen"]));
											end; twait(0.7)
											Tp(FindFirstChild(R, "Cake Queen").HumanoidRootPart.CFrame * CF(0,35,0), Configs.Settings.AutoChest.TP_Speed or 3);
											twait(1.3);
											pcal(function()
												for i,v in pir(Enem) do
													if v.Name == "Cake Queen" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															sethiddenproperty(selff,"SimulationRadius",math.huge);
															repeat task.wait(); TargetMobName = v.Name;
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed or 3);
																SetHum(v); Hitc();
															until not v or not Configs.Farming.Sea3.AutoBuddySword or v.Humanoid.Health <= 0
														end;
													end;
												end;
											end);
										else
											--[[
											if Configs.Farming.Sea1.Hop then
												task.wait(3);
												if (not FindFirstChild(R, "Cake Queen") or not FindFirstChild(Enemy, "Saber Expert")) and Configs.Farming.Sea1.Hop and not Configs.Settings.AutoFarm.Stackable then
													Hop();
												end;
											end;
											--]]
										end;
									end);
								end;
							end;
							if Configs.Farming.Sea3.AutoCarvender and not getInventory("Canvander", "Check") then
								if FindFirstChild(Enemy, "Beautiful Pirate") then
									pcal(function()
										for i,v in pir(Enem) do
											if v.Name == "Beautiful Pirate" then
												if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
													sethiddenproperty(selff,"SimulationRadius",math.huge);
													repeat twait(); TargetMobName = v.Name;
														Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed or 3);
														SetHum(v); Hitc();
													until not v or not Configs.Farming.Sea3.AutoCarvender or v.Humanoid.Health <= 0
												end;
											end;
										end;
									end);
								else
									pcal(function()
										if FindFirstChild(R, "Beautiful Pirate") then
											if Configs.Teleport.TP_Bypass and R["Beautiful Pirate"] then
												TpBypass(GetPivot(R["Beautiful Pirate"]));
											end; twait(0.7)
											Tp(FindFirstChild(R, "Beautiful Pirate").HumanoidRootPart.CFrame * CF(0,35,0), Configs.Settings.AutoChest.TP_Speed or 3);
											twait(1.3);
											pcal(function()
												for i,v in pir(Enem) do
													if v.Name == "Beautiful Pirate" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															sethiddenproperty(selff,"SimulationRadius",math.huge);
															repeat task.wait(); TargetMobName = v.Name;
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															until not v or not Configs.Farming.Sea3.AutoCarvender or v.Humanoid.Health <= 0
														end;
													end;
												end;
											end);
										else
											if Configs.Farming.Sea3.AutoCarvenderHop then
												task.wait(3);
												if (not FindFirstChild(R, "Beautiful Pirate") or not FindFirstChild(Enemy, "Beautiful Pirate")) and Configs.Farming.Sea3.AutoCarvenderHop then
													Hop();
												end;
											end;
										end;
									end);
								end;
							end;
							if Configs.Farming.Sea3.AutoKillDoughKing then
								pcall(function()
									if FindFirstChild(Enemy, "Dough King") then
										for i, v in pir(Enemy:GetChildren()) do
											if v.Name == "Dough King" then
												if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
													repeat twait();
														if dist(GetPivot(v).Position) >= 3000 then
															if dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) < 1000 then
																firetouchinterest(W.Map.CakeLoaf.BigMirror.Main.TouchInterest, 0); twait(1);
															end;
														end;
														Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
														SetHum(v); Hitc();
													until not Configs.Farming.Sea3.AutoKillDoughKing or not v or not v.Parent or v.Humanoid.Health <= 0;
												end;
											end;
										end;
									else
										if FindFirstChild(R, "Dough King") then
											if Configs.Teleport.TP_Bypass and dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) >= 3000 then
												TpBypass(CF(-1976.0748291015625, 37.92359924316406, -11981.326171875));
												twait(0.7);
											end;
											if FindFirstChild(R, "Dough King") then
												Tp(CF(-1976.0748291015625, 37.92359924316406, -11981.326171875), Configs.Settings.AutoFarm.TP_Speed);
											end;
											if dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) < 1000 then
												firetouchinterest(W.Map.CakeLoaf.BigMirror.Main.TouchInterest, 0); twait(1);
											end;
											if FindFirstChild(Enemy, "Dough King") then
												for i, v in pir(Enemy:GetChildren()) do
													if v.Name == "Dough King" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															repeat twait();
																if dist(GetPivot(v).Position) >= 3000 then
																	if dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) < 1000 then
																		firetouchinterest(W.Map.CakeLoaf.BigMirror.Main.TouchInterest, 0); twait(1);
																	end;
																end;
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															until not Configs.Farming.Sea3.AutoKillDoughKing or not v or not v.Parent or v.Humanoid.Health <= 0;
														end;
													end;
												end;
											else
												if dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) < 1000 then
													firetouchinterest(W.Map.CakeLoaf.BigMirror.Main.TouchInterest, 0);
													twait(1)
												end;
												for i, v in pir(Enemy:GetChildren()) do
													if v.Name == "Dough King" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															repeat twait();
																if dist(GetPivot(v).Position) >= 3000 then
																	if dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) < 1000 then
																		firetouchinterest(W.Map.CakeLoaf.BigMirror.Main.TouchInterest, 0);
																	end;
																end;
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															until not Configs.Farming.Sea3.AutoKillDoughKing or not v or not v.Parent or v.Humanoid.Health <= 0;
														end;
													end;
												end;
											end;
										end;
									end;
								end);
							end;
							if Configs.Farming.Sea3.AutoElitehunter then
								if IsEliteHunterSpawn then
									pcal(function()
										repeat twait();
											QuestTitle = PSG.Main.Quest.Container.QuestTitle.Title.Text;
											if PSG.Main.Quest.Visible then
												if strfind(QuestTitle, "Diablo") or strfind(QuestTitle, "Deandre") or strfind(QuestTitle, "Urban") then
													if (FindFirstChild(Enemy, "Diablo") or FindFirstChild(Enemy, "Deandre") or FindFirstChild(Enemy, "Urban")) then
														for _, v in pir(GetChildren(Enemy)) do
															if v and (v.Name == "Diablo" or v.Name == "Deandre" or v.Name == "Urban") and FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															end;
														end;
													else
														WhereisEliteBruh = FindFirstChild(R, "Diablo") or FindFirstChild(R, "Deandre") or FindFirstChild(R, "Urban") or nil;
														if Configs.Teleport.TP_Bypass then
															TpBypass(WhereisEliteBruh:GetPivot());
															twait(0.7);
														end;
														if WhereisEliteBruh then
															Tp(WhereisEliteBruh:GetPivot() * CF(35, 35, 0), Configs.Settings.AutoFarm.TP_Speed);
														end;
													end;
												else
													if PSG.Main.Quest.Visible then
														CommF:InvokeServer("AbandonQuest");
													end;
													if Configs.Teleport.TP_Bypass and dist(Vec3(-5418.892578125, 313.74130249023, -2826.2260742188)) > 3000 then
														TpBypass(CF(-5418.892578125, 313.74130249023, -2826.2260742188));
														twait(0.7);
													end;
													repeat twait();
														Tp(CF(-5418.892578125, 313.74130249023, -2826.2260742188), Configs.Settings.AutoFarm.TP_Speed);
													until dist(Vec3(-5418.892578125, 313.74130249023, -2826.2260742188)) <= 5 or not Configs.Farming.Sea3.AutoElitehunter or not IsEliteHunterSpawn;
													if dist(Vec3(-5418.892578125, 313.74130249023, -2826.2260742188)) <= 5 and Configs.Farming.Sea3.AutoElitehunter and IsEliteHunterSpawn then
														CommF:InvokeServer("EliteHunter");
													end;
												end;
											else
												if Configs.Teleport.TP_Bypass and dist(Vec3(-5418.892578125, 313.74130249023, -2826.2260742188)) > 3000 then
													TpBypass(CheckLevel()[6]);
													twait(0.7);
												end;
												repeat twait();
													Tp(CF(-5418.892578125, 313.74130249023, -2826.2260742188), Configs.Settings.AutoFarm.TP_Speed);
												until dist(Vec3(-5418.892578125, 313.74130249023, -2826.2260742188)) <= 5 or not Configs.Farming.Sea3.AutoElitehunter or not IsEliteHunterSpawn;
												if dist(Vec3(-5418.892578125, 313.74130249023, -2826.2260742188)) <= 5 and Configs.Farming.Sea3.AutoElitehunter and IsEliteHunterSpawn then
													CommF:InvokeServer("EliteHunter");
												end;
											end;
										until not IsEliteHunterSpawn or not Configs.Farming.Sea3.AutoElitehunter
									end);
								end;
							end;
							if Configs.Farming.Sea3.AutoCakePrince then
								pcall(function()
									if FindFirstChild(Enemy, "Cake Prince") then
										for i, v in pir(Enemy:GetChildren()) do
											if v.Name == "Cake Prince" then
												if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
													repeat twait();
														if dist(GetPivot(v).Position) >= 3000 then
															if dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) < 1000 then
																firetouchinterest(W.Map.CakeLoaf.BigMirror.Main.TouchInterest, 0); twait(1);
															end;
														end;
														Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
														SetHum(v); Hitc();
													until not Configs.Farming.Sea3.AutoKillDoughKing or not v or not v.Parent or v.Humanoid.Health <= 0;
												end;
											end;
										end;
									else
										if FindFirstChild(R, "Cake Prince") then
											if Configs.Teleport.TP_Bypass and dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) >= 3000 then
												TpBypass(CF(-1976.0748291015625, 37.92359924316406, -11981.326171875));
												twait(0.7);
											end;
											if FindFirstChild(R, "Cake Prince") then
												Tp(CF(-1976.0748291015625, 37.92359924316406, -11981.326171875), Configs.Settings.AutoFarm.TP_Speed);
											end;
											if dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) < 1000 then
												firetouchinterest(W.Map.CakeLoaf.BigMirror.Main.TouchInterest, 0); twait(1);
											end;
											if FindFirstChild(Enemy, "Cake Prince") then
												for i, v in pir(Enemy:GetChildren()) do
													if v.Name == "Cake Prince" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															repeat twait();
																if dist(GetPivot(v).Position) >= 3000 then
																	if dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) < 1000 then
																		firetouchinterest(W.Map.CakeLoaf.BigMirror.Main.TouchInterest, 0); twait(1);
																	end;
																end;
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															until not Configs.Farming.Sea3.AutoKillDoughKing or not v or not v.Parent or v.Humanoid.Health <= 0;
														end;
													end;
												end;
											else
												if dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) < 1000 then
													firetouchinterest(W.Map.CakeLoaf.BigMirror.Main.TouchInterest, 0);
													twait(1)
												end;
												for i, v in pir(Enemy:GetChildren()) do
													if v.Name == "Cake Prince" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															repeat twait();
																if dist(GetPivot(v).Position) >= 3000 then
																	if dist(-1976.0748291015625, 37.92359924316406, -11981.326171875) < 1000 then
																		firetouchinterest(W.Map.CakeLoaf.BigMirror.Main.TouchInterest, 0);
																	end;
																end;
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed or 3);
																SetHum(v); Hitc();
															until not Configs.Farming.Sea3.AutoKillDoughKing or not v or not v.Parent or v.Humanoid.Health <= 0;
														end;
													end;
												end;
											end;
										end;
									end;
								end);
							end;
						end;
						if Configs.Farming.Sea3.FullAutoAzureEmber and GG.FullAutoAzureEmber then
							function MadeForUnique___3424(MadeForUnique___3424Name)
								MadeForUnique___3424Var = CommF:InvokeServer("getInventory");
								for i, v in pir(MadeForUnique___3424Var) do
									if v.Name == MadeForUnique___3424Name then
										return v.Count;
									end;
								end;
							end;
							if FindFirstChild(W.Map, "KitsuneIsland") and GG.FullAutoAzureEmber then
								repeat twait();
									if dist(W.Map.KitsuneIsland.ShrineActive.NeonShrinePart.Position) > 1000 then
										Tp(W.Map.KitsuneIsland.ShrineActive.NeonShrinePart.CFrame * CFrame.new(0, 0, 10), Configs.Settings.AutoFarm.TP_Speed);
									else
										if FindFirstChild(W, "AttachedAzureEmber") then
											Tp(W:WaitForChild("EmberTemplate"):FindFirstChild("Part").CFrame, 5);
										end;
									end;
									if MadeForUnique___3424("Azure Ember") >= 20 then
										FindFirstChild(R.Modules.Net, "RF/KitsuneStatuePray"):InvokeServer();
										CommF:InvokeServer("KitsuneStatuePray");
										GG.FullAutoAzureEmber = false;
										tspawn(function()
											repeat twait() until not FindFirstChild(W.Map, "KitsuneIsland");
											GG.FullAutoAzureEmber = true;
										end);
									end;
								until not FindFirstChild(W.Map, "KitsuneIsland") or not Configs.Farming.Sea3.FullAutoAzureEmber or not GG.FullAutoAzureEmber
							end;
						end;
						if Configs.Farming.Sea3.AutoFarmBone then
							pcal(function()
								QuestTitle = PSG.Main.Quest.Container.QuestTitle.Title.Text;
								if not strfind(QuestTitle, "Demonic Soul") then
									CommF:InvokeServer("AbandonQuest");
								end;
								if not PSG.Main.Quest.Visible then
									if Configs.Teleport.TP_Bypass and dist(Vec3(-9516.99316, 172.017181, 6078.46533)) >= 3000 then
										TpBypass(CF(-9516.99316, 172.017181, 6078.46533));
										twait(0.7);
									end;
									Tp(CF(-9516.99316, 172.017181, 6078.46533), Configs.Settings.AutoChest.TP_Speed or 4);
									if dist(Vec3(-9516.99316, 172.017181, 6078.46533)) <= 5 then
										CommF:InvokeServer("StartQuest", "HauntedQuest2", 1);
									end;
								else
									if FindFirstChild(Enemy, "Reborn Skeleton") or FindFirstChild(Enemy, "Living Zombie") or FindFirstChild(Enemy, "Demonic Soul") or FindFirstChild(Enemy, "Posessed Mummy") then
										for i, v in pir(Enem) do
											if FindFirstChild(v, "HumanoidRootPart") and FindFirstChild(v, "Humanoid") and v.Humanoid.Health > 0 then
												if v.Name == "Reborn Skeleton" or v.Name == "Living Zombie" or v.Name == "Demonic Soul" or v.Name == "Posessed Mummy" then
													if strfind(PSG.Main.Quest.Container.QuestTitle.Title.Text, "Demonic Soul") then
														TargetMobName = v.Name
														repeat twait();
															Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
															SetHum(v); Hitc();
														until not Configs.Farming.Sea3.AutoFarmBone or v.Humanoid.Health <= 0 or (not v.Parent) or not PSG.Main.Quest.Visible;
													end;
												end;
											end;
										end;
									else
										RepFindBoneMobs = FindFirstChild(R, "Reborn Skeleton") or FindFirstChild(R, "Living Zombie") or FindFirstChild(R, "Demonic Soul") or FindFirstChild(R, "Posessed Mummy");
										if RepFindBoneMobs then
											if Configs.Teleport.TP_Bypass and dist(Vec3(-9516.99316, 172.017181, 6078.46533)) >= 3000 then
												TpBypass(CF(-9516.99316, 172.017181, 6078.46533));
												twait(0.7);
											end;
											Tp(GetPivot(RepFindBoneMobs) * CF(35, 35, 0), Configs.Settings.AutoFarm.TP_Speed);
										end;
									end;
								end;
							end);
						end;
					end;
					if Configs.Farming.Level.AutoFarm and (not Configs.Farming.Sea3.AutoFarmBone) then
						pcal(function()
							Level = Data.Level.Value;
							if Level >= 700 and WorldCheck["First Sea"] then
								if Configs.Farming.Level.AutoSea2 then
									OnFocusSea2 = true;
									IgnoreE = true;
									if W.Map.Ice.Door.CanCollide and W.Map.Ice.Door.Transparency == 0 then
										repeat
											Tp(CF(4849.29883, 5.65138149, 719.611877), Configs.Settings.AutoFarm.TP_Speed);
											twait();
										until dist(Vec3(4849.29883, 5.65138149, 719.611877)) <= 3 or not Configs.Farming.Level.AutoSea2;
										wait(1.1);
										CommF:InvokeServer("DressrosaQuestProgress", "Detective");
										wait(0.5);
										EquipWeapon("Key");
										repeat
											Tp(CF(1347.7124, 37.3751602, -1325.6488), Configs.Settings.AutoFarm.TP_Speed);
											wait();
										until dist(Vec3(1347.7124, 37.3751602, -1325.6488)) <= 3 or not Configs.Farming.Level.AutoSea2;
										wait(0.5);
										if not W.Map.Ice.Door.CanCollide and W.Map.Ice.Door.Transparency == 1 then
											if FindFirstChild(Enemy, "Ice Admiral") then
												for i, v in pir(Enemy:GetChildren()) do
													if v.Name == "Ice Admiral" and Configs.Farming.Level.AutoSea2 then
														if FindFirstChild(v, "HumanoidRootPart") and FindFirstChild(v, "Humanoid") and v.Humanoid.Health > 0 then
															repeat twait();
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															until not v or v.Humanoid.Health <= 0 or not Configs.Farming.Level.AutoSea2
														else
															CommF:InvokeServer("TravelDressrosa");
														end;
													end;
												end;
											end;
										end;
									elseif not W.Map.Ice.Door.CanCollide and W.Map.Ice.Door.Transparency == 1 then
										if FindFirstChild(Enemy, "Ice Admiral") then
											for i, v in pir(Enemy:GetChildren()) do
												if v.Name == "Ice Admiral" and Configs.Farming.Level.AutoSea2 then
													if FindFirstChild(v, "HumanoidRootPart") and FindFirstChild(v, "Humanoid") and v.Humanoid.Health > 0 then
														repeat twait();
															Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
															SetHum(v); Hitc();
														until not v or v.Humanoid.Health <= 0 or not Configs.Farming.Level.AutoSea2
													else
														CommF:InvokeServer("TravelDressrosa");
													end;
												end;
											end;
										elseif FindFirstChild(R, "Ice Admiral") then
											Tp(FindFirstChild(R, "Ice Admiral").HumanoidRootPart.CFrame * CFrame.new(0, 35, 0), Configs.Settings.AutoFarm.TP_Speed);
											twait(1.5);
											for i, v in pir(Enemy:GetChildren()) do
												if v.Name == "Ice Admiral" and Configs.Farming.Level.AutoSea2 then
													if FindFirstChild(v, "HumanoidRootPart") and FindFirstChild(v, "Humanoid") and v.Humanoid.Health > 0 then
														repeat twait();
															Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
															SetHum(v); Hitc(); EquipWeapon(Configs.Farming.Weapon);
														until not v or v.Humanoid.Health <= 0 or not Configs.Farming.Level.AutoSea2
													else
														CommF:InvokeServer("TravelDressrosa");
													end;
												end;
											end;
										end;
									end;
									CommF:InvokeServer("TravelDressrosa");
								else
									OnFocusSea2 = false;
								end;
								IgnoreE = false;
							end;
						end);
						pcal(function()
							if Level >= 1500 and WorldCheck["Second Sea"] then
								if Configs.Farming.Level.AutoSea3 then
									OnFocusSea3 = true;
									if PData.DidBartilo then
										if CommF:InvokeServer("ZQuestProgress", "General") == 0 then
											repeat twait();
												Tp(CF(-1926.3221435547, 12.819851875305, 1738.3092041016), Configs.Settings.AutoFarm.TP_Speed);
											until dist(Vec3(-1926.3221435547, 12.819851875305, 1738.3092041016)) <= 10 or not Configs.Farming.Level.AutoSea3;
											if dist(Vec3(-1926.3221435547, 12.819851875305, 1738.3092041016)) <= 10 then
												twait(1.5);
												CommF:InvokeServer("ZQuestProgress", "Begin");
											end;
											SpawnAttemp = 0;
											repeat
												twait();
												SpawnAttemp = SpawnAttemp + 1;
											until not Configs.Farming.Level.AutoSea3 or FindFirstChild(Enemy, "rip_indra") or SpawnAttemp == 1000;
											if FindFirstChild(Enemy, "rip_indra") then
												for i,v in pir(Enemy:GetChildren()) do
													if v.Name == "rip_indra" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															sethiddenproperty(selff,"SimulationRadius",math.huge);
															repeat twait();
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															until not v or not Configs.Farming.Level.AutoSea3 or v.Humanoid.Health <= 0
														end;
													end;
												end;
											end;
										elseif CommF:InvokeServer("ZQuestProgress", "General") == nil then
											Notification.new({
												Description = "You have to defeat Don Swarn first!!!";
												Title = "Neuron X [Guide]";
												Duration = 5;
											});
										end; twait(2);
										if not strfind(CommF:InvokeServer("TravelZou"), "combat") then
											CommF:InvokeServer("TravelZou")
										else
											if FindFirstChild(Enemy, "rip_indra") and dist(FindFirstChild(Enemy, "rip_indra"):GetPivot().Position) <= 500 then
												for i,v in pir(Enemy:GetChildren()) do
													if v.Name == "rip_indra" then
														if FindFirstChild(v, "Humanoid") and FindFirstChild(v, "HumanoidRootPart") and v.Humanoid.Health > 0 then
															sethiddenproperty(selff,"SimulationRadius",math.huge);
															repeat twait();
																if dist(FindFirstChild(Enemy, "rip_indra"):GetPivot().Position) >= 500 then
																	Configs.System.BREAKALLTHISSHITHAHAHAHAHA = true; twait(3); Configs.System.BREAKALLTHISSHITHAHAHAHAHA = false;
																	break;
																end;
																Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
																SetHum(v); Hitc();
															until not v or not Configs.Farming.Level.AutoSea3 or v.Humanoid.Health <= 0
														end;
													end;
												end;
											end;
										end;
									else
										Configs.Farming.Sea2.AutoBartilo = true;
										ABTLITOAQUET.Value(true);
									end;
								else
									OnFocusSea3 = false;
								end;
							end;
						end);
						PcallCheck, ErrorCheckNoodle = pcal(function()
							if not OnFocusSea3 and not OnFocusSea2 then
								IgnoreE = false;
								TargetMobName = CheckLevel()[4]; twait(0.175);
								if TargetMobName == "Demonic Souls" then
									TargetMobName = "Demonic Soul";
								elseif TargetMobName == "Posessed Mummys" then
									TargetMobName = "Posessed Mummy";
								end;
								if Configs.Teleport.TP_Bypass and dist(CheckLevel()[6].Position) > 3000 then
									TpBypass(CheckLevel()[6]);
									twait(0.7);
								end;
								QuestTitle = PSG.Main.Quest.Container.QuestTitle.Title.Text;
								if not strfind(QuestTitle, CheckLevel()[4]) then
									CommF:InvokeServer("AbandonQuest");
								end;
								if not PSG.Main.Quest.Visible then
									repeat twait(); Tp(CheckLevel()[5], Configs.Settings.AutoFarm.TP_Speed); until dist(CheckLevel()[5].Position) <= 3 or not Configs.Farming.Level.AutoFarm
									if dist(CheckLevel()[5].Position) <= 3 and Configs.Farming.Level.AutoFarm then
										twait(1.2);
										CommF:InvokeServer("StartQuest", CheckLevel()[2], CheckLevel()[3]);
										twait(0.5);
									end;
								end;
								twait();
								if FindFirstChild(Enemy, TargetMobName) then
									for i,v in pir(Enem) do
										if v and Configs.Farming.Level.AutoFarm and FindFirstChild(v, "HumanoidRootPart") and FindFirstChild(v, "Humanoid") and v.Humanoid.Health > 0 and PSG.Main.Quest.Visible then
											if v.Name == TargetMobName then
												if strfind(PSG.Main.Quest.Container.QuestTitle.Title.Text, CheckLevel()[4]) then
													repeat twait();
														if Configs.Teleport.TP_Bypass and dist(CheckLevel()[6].Position) > 3000 then
															TpBypass(CheckLevel()[6]);
															twait(0.7)
														end;
														Tp(v.HumanoidRootPart.CFrame * CF(Configs.Settings.AutoFarm.PosX, Configs.Settings.AutoFarm.PosY, Configs.Settings.AutoFarm.PosZ), Configs.Settings.AutoFarm.TP_Speed);
														SetHum(v); Hitc();
													until not v or not Configs.Farming.Level.AutoFarm or v.Humanoid.Health <= 0 or not PSG.Main.Quest.Visible
												else
													CommF:InvokeServer("AbandonQuest");
												end;
											end;
										end;
									end;
								else
									if FindFirstChild(R, TargetMobName) then
										Tp(GetPivot(FindFirstChild(R, TargetMobName)) * CF(0, 35, 0), Configs.Settings.AutoFarm.TP_Speed);
									else
										if dist(CheckLevel()[6].Position) > 70 then
											Tp(CheckLevel()[6] * CF(0,35,0), Configs.Settings.AutoFarm.TP_Speed);
										end;
									end;
								end;
							end;
						end);
						if not PcallCheck then
							warn(ErrorCheckNoodle);
						end;
					else
						if Configs.Farming.Level.AutoFarm then
							if Configs.Farming.Sea3.AutoFarmBone then
								Notification.new({
									Description = "You can't turn on both Farm Level and Farm Bone at the same time";
									Title = "Neuron X [Stack]";
									Duration = 10;
								});
								twait(3);
							end;
						end;
					end;
				--end);
			end;
		end);
	end;
	if Iden == "Solara" then
		Notification.new({
			Description = "Your current Executor is Solara or Solara Skidded and may not fully support this script. You should use HitBox Expander for Auto Farm";
			Title = "Neuron X Executor";
			Duration = 10;
		});
	end;

	Notification.new({
		Description = "Updated";
		Title = "Neuron X Data";
		Duration = 10;
	});

	Notification.new({
		Description = "System force use deep check for stackable auto farm which cause long condition on checking each progress.";
		Title = "Neuron X System";
		Duration = 10;
	});

	Notification.new({
		Description = "Client link is not ready for this bloxfruit version.";
		Title = "Neuron X Link";
		Duration = 10;
	});

	Notification.new({
		Description = "UNC is prepared as " .. Iden;
		Title = "Neuron X Executor";
		Duration = 3;
	});

	Notification.new({
		Description = "If the functions doesn't work we want to let you know that its not a bug but sometimes you need to wait.";
		Title = "Neuron X Guide";
		Duration = 10;
	});
end;

warn("[Neuron X] : Script is running on different thread");
warn("[Neuron X] : Script is running on <5583x16>");

return coroutine.wrap(getMethodLoading)();
