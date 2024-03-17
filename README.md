local drizzlelib = loadstring(game:HttpGet("https://raw.githubusercontent.com/DRIZZLEHUB/drizzleLibV5/main/Source.Lua"))()
local Window = drizzlelib:MakeWindow({
  Title = "drizzle Hub : Blox Fruits",
  SubTitle = "by chuvisco",
  SaveFolder = "drizzle Hub | Blox Fruits.lua"
})

local AFKOptions = {}

local Discord = Window:MakeTab({"Discord", "Info"})
Discord:AddDiscordInvite({
  Name = "Herdroock Hub | Community",
  Description = "Join our discord community to receive information about the next update",
  Logo = "rbxassetid://15298567397",
  Invite = "https://discord.com/invite/Hw5VDXez3V"
})
local MainFarm = Window:MakeTab({"Farm", "Home"})
if Sea3 then
  local AutoSea = Window:MakeTab({"Sea", "Waves"})
  AutoSea:AddSection({"Kitsune"})
  local KILabel = AutoSea:AddParagraph({"Kitsune Island : not spawn"})
  AutoSea:AddToggle({Name = "Auto Kitsune Island",Callback = function(Value)
    getgenv().AutoKitsuneIsland = Value;AutoKitsuneIsland()
  end})
  AutoSea:AddToggle({Name = "Auto Trade Azure Ember",Callback = function(Value)
    getgenv().TradeAzureEmber = Value
    task.spawn(function()
      local Modules = ReplicatedStorage:WaitForChild("Modules", 9e9)
      local Net = Modules:WaitForChild("Net", 9e9)
      local KitsuneRemote = Net:WaitForChild("RF/KitsuneStatuePray", 9e9)
      
      while getgenv().TradeAzureEmber do task.wait(1)
        KitsuneRemote:InvokeServer()
      end
    end)
  end})
  task.spawn(function()
    local Map = workspace:WaitForChild("Map", 9e9)
    task.spawn(function()
      while task.wait() do
        if Map:FindFirstChild("KitsuneIsland") then
          local plrPP = Player.Character and Player.Character.PrimaryPart
          if plrPP then
            Distance = tostring(math.floor((plrPP.Position - Map.KitsuneIsland.WorldPivot.p).Magnitude / 3))
          end
        end
      end
    end)
    
    while task.wait() do
      if Map:FindFirstChild("KitsuneIsland") then
        KILabel:SetTitle("Kitsune Island : Spawned | Distance : " .. Distance)
      else
        KILabel:SetTitle("Kitsune Island : not Spawn")
      end
    end
  end)
  AutoSea:AddSection({"Sea"})
  AutoSea:AddToggle({Name = "Auto Farm Sea",Callback = function(Value)
    getgenv().AutoFarmSea = Value;AutoFarmSea()
  end})
  AutoSea:AddButton({Name = "Buy New Boat",Callback = function()
    BuyNewBoat()
  end})
  AutoSea:AddSection({"Material"})
  AutoSea:AddToggle({"Auto Wood Planks", false, function(Value)
    getgenv().AutoWoodPlanks = Value
    task.spawn(function()
      local Map = workspace:WaitForChild("Map", 9e9)
      local BoatCastle = Map:WaitForChild("Boat Castle", 9e9)
      
      local function TreeModel()
        for _,Model in pairs(BoatCastle["IslandModel"]:GetChildren()) do
          if Model.Name == "Model" and Model:FindFirstChild("Tree") then
            return Model
          end
        end
      end
      
      local function GetTree()
        local Tree = TreeModel()
        if Tree then
          local Nearest = math.huge
          local selected
          for _,tree in pairs(Tree:GetChildren()) do
            local plrPP = Player.Character and Player.Character.PrimaryPart
            if tree and tree.PrimaryPart and tree.PrimaryPart.Anchored then
              if plrPP and (plrPP.Position - tree.PrimaryPart.Position).Magnitude < Nearest then
                Nearest = (plrPP.Position - tree.PrimaryPart.Position).Magnitude
                selected = tree
              end
            end
          end
          return selected
        end
      end
      
      local RandomEquip = ""
      task.spawn(function()
        while getgenv().AutoWoodPlanks do
          if VerifyToolTip("Melee") then
            RandomEquip = "Melee"task.wait(2)
          end
          if VerifyToolTip("Blox Fruit") then
            RandomEquip = "Blox Fruit"task.wait(3)
          end
          if VerifyToolTip("Sword") then
            RandomEquip = "Sword"task.wait(2)
          end
          if VerifyToolTip("Gun") then
            RandomEquip = "Gun"task.wait(2)
          end
        end
      end)
      
      while getgenv().AutoWoodPlanks do task.wait()
        local Tree = GetTree()EquipToolTip(RandomEquip)
        
        if Tree and Tree.PrimaryPart then
          PlayerTP(Tree.PrimaryPart.CFrame)
          local plrPP = Player.Character and Player.Character.PrimaryPart
          if plrPP and (plrPP.Position - Tree.PrimaryPart.Position).Magnitude < 10 then
            if getgenv().SeaSkillZ then
              KeyboardPress("Z")
            end
            if getgenv().SeaSkillX then
              KeyboardPress("X")
            end
            if getgenv().SeaSkillC then
              KeyboardPress("C")
            end
            if getgenv().SeaSkillV then
              KeyboardPress("V")
            end
            if getgenv().SeaSkillF then
              KeyboardPress("F")
            end
            if getgenv().SeaAimBotSkill then
              AimBotPart(Tree.PrimaryPart)
            end
          end
        end
      end
    end)
  end})
  AutoSea:AddSection({"Panic Mode"})
  AutoSea:AddSlider({Name = "Select Health",Min = 20,Max = 70,Default = 25,Callback = function(Value)
    getgenv().HealthPanic = Value
  end})
  AutoSea:AddToggle({"Panic Mode", true, function(Value)
    getgenv().PanicMode = Value
  end, "Sea/PanicMode"})
  AutoSea:AddSection({"Farm Select"})
  AutoSea:AddParagraph({"Fish"})
  AutoSea:AddToggle({Name = "Terrorshark", Flag = "Sea/TerrorShark", Default = true,Callback = function(Value)
    getgenv().Terrorshark = Value
  end})
  AutoSea:AddToggle({Name = "Piranha", Flag = "Sea/Piranha", Default = true,Callback = function(Value)
    getgenv().Piranha = Value
  end})
  AutoSea:AddToggle({Name = "Fish Crew Member", Flag = "Sea/FishCrewMember", Default = true,Callback = function(Value)
    getgenv().FishCrewMember = Value
  end})
  AutoSea:AddToggle({Name = "Shark", Flag = "Sea/Shark", Default = true,Callback = function(Value)
    getgenv().Shark = Value
  end})
  AutoSea:AddParagraph({"Boats"})
  AutoSea:AddToggle({Name = "Pirate Brigade", Flag = "Sea/PirateBrigade", Default = true,Callback = function(Value)
    getgenv().PirateBrigade = Value
  end})
  AutoSea:AddToggle({Name = "Pirate Grand Brigade", Flag = "Sea/PirateGrandBrigade", Default = true,Callback = function(Value)
    getgenv().PirateGrandBrigade = Value
  end})
  AutoSea:AddToggle({Name = "Fish Boat", Flag = "Sea/FishBoat", Default = true,Callback = function(Value)
    getgenv().FishBoat = Value
  end})
  --[[AddTextLabel(AutoSea, {"Sea Beast"})
  AutoSea:AddToggle({Name = "Sea Beast",Default = true,Callback = function(Value)
    getgenv().SeaBeast = Value
  end})
  AutoSea:AddToggle({Name = "Triple Sea Beast",Default = true,Callback = function(Value)
    getgenv().SeaBeastTriple = Value
  end})]]
  AutoSea:AddSection({"Skill"})
  AutoSea:AddToggle({Name = "AimBot Skill Enemie", Flag = "Mastery/Aimbot", Default = true,Callback = function(Value)
    getgenv().SeaAimBotSkill = Value
  end})
  AutoSea:AddToggle({Name = "Skill Z", Flag = "Mastery/Z", Default = true,Callback = function(Value)
    getgenv().SeaSkillZ = Value
  end})
  AutoSea:AddToggle({Name = "Skill X", Flag = "Mastery/X", Default = true,Callback = function(Value)
    getgenv().SeaSkillX = Value
  end})
  AutoSea:AddToggle({Name = "Skill C", Flag = "Mastery/C", Default = true,Callback = function(Value)
    getgenv().SeaSkillC = Value
  end})
  AutoSea:AddToggle({Name = "Skill V", Flag = "Mastery/V", Default = true,Callback = function(Value)
    getgenv().SeaSkillV = Value
  end})
  AutoSea:AddToggle({Name = "Skill F", Flag = "Mastery/F", Callback = function(Value)
    getgenv().SeaSkillF = Value
  end})
  AutoSea:AddSection({"NPCs"})
  AutoSea:AddToggle({Name = "Teleport To Shark Hunter",Callback = function(Value)
    getgenv().NPCtween = Value
    task.spawn(function()
      while getgenv().NPCtween do task.wait()
        PlayerTP(CFrame.new(-16526, 108, 752))
      end
    end)
  end})
  AutoSea:AddToggle({Name = "Teleport To Beast Hunter",Callback = function(Value)
    getgenv().NPCtween = Value
    task.spawn(function()
      while getgenv().NPCtween do task.wait()
        PlayerTP(CFrame.new(-16281, 73, 263))
      end
    end)
  end})
  AutoSea:AddToggle({Name = "Teleport To Spy",Callback = function(Value)
    getgenv().NPCtween = Value
    task.spawn(function()
      while getgenv().NPCtween do task.wait()
        PlayerTP(CFrame.new(-16471, 528, 539))
      end
    end)
  end})
  AutoSea:AddSection({"Configs"})
  AutoSea:AddDropdown({
    Name = "Tween Sea Level",
    Options = {"1", "2", "3", "4", "5", "6", "inf"},
    Default = {"6"},
    Flag = "Sea/SeaLevel",
    Callback = function(Value)
      getgenv().SeaLevelTP = Value
    end
  })
  AutoSea:AddSlider({
    Name = "Boat Tween Speed",
    Min = 100,
    Max = 300,
    Increase = 10,
    Default = 250,
    Flag = "Sea/BoatSpeed",
    Callback = function(Value)
      getgenv().SeaBoatSpeed = Value
    end
  })
end
if Sea3 and IsOwner then
  local MirageTab = Window:MakeTab({"Race V4", ""})
  
  MirageTab:AddToggle({"Auto Pull Lever", false, function(Value)
    
  end})
  
  MirageTab:AddToggle({"Auto Stone Puzzle", false, function(Value)
    
  end})
  
  MirageTab:AddSection({"Auto Mirage"})
  
  MirageTab:AddToggle({"Auto Find Mirage", false, function(Value)
    
  end})
  
  MirageTab:AddToggle({"Auto Gear Puzzle", false, function(Value)
    getgenv().AutoMiragePuzzle = Value
    
    local function GetGear()
      
    end
    
    local function LookToMoon()
      local MoonDirection = Lighting:GetMoonDirection()
      local LookToPos = Camera.CFrame.p + moonDir * 100
      Camera.CFrame = CFrame.lookAt(Camera.CFrame.p, LookToPos0)
    end
    
    local Connection = RunService.Heartbeat:Connect(LookToMoon)
    while getgenv().AutoMiragePuzzle do task.wait()end
    if Connection then
      Connection:Disconnect()
    end
  end})
  
  MirageTab:AddSection({"Auto Race"})
  
  MirageTab:AddToggle({"Auto Finish Trial", false, function(Value)
    getgenv().AutoFinishTrial = Value
    task.spawn(function()
      local PlayerRace
      task.spawn(function()
        while getgenv().AutoFinishTrial do task.wait()
          PlayerRace = Player.Data.Race.Value
        end
      end)
      
      while getgenv().AutoFinishTrial do task.wait()
        if PlayerRace and typeof(PlayerRace) == "string" then
          if PlayerRace == "Cyborg" then
            PlayerTP(CFrame.new(28654, 14898, -30))
          elseif PlayerRace == "Ghoul" then
            KillAura()
          elseif PlayerRace == "Human" then
            KillAura()
          elseif PlayerRace == "Mink" then
            for _,part in pairs(workspace:GetDescendants()) do
              if part.Name == "StartPoint" then
                PlayerTP(part.CFrame)
              end
            end
          elseif PlayerRace == "Skypiea" then
            pcall(function()
              for _,part in pairs(workspace.Map.SkyTrial.Model:GetDescendants()) do
                if part.Name ==  "snowisland_Cylinder.081" then
                  PlayerTP(part.CFrame)
                end
              end
            end)
          end
        end
      end
    end)
  end})
end
local QuestsTabs = Window:MakeTab({"Quests/Items", "Swords"})
local FruitAndRaid = Window:MakeTab({"Fruit/Raid", "Cherry"})
--[[local Informacoes = Window:MakeTab({"Info", "Search"})
Informacoes:AddSection({"Player"})
local Nerd = Informacoes:AddParagraph({"Nerd < Accessories Info >"})
task.spawn(function()
  while task.wait() do
    Nerd:SetDesc(FireRemote("Nerd"))
  end
end)]]
if PlayerLevel.Value < MaxLavel then
  local StatsTab = Window:MakeTab({"Stats", "signal"})
  local PointsSlider, Melee, Defense, Sword, Gun, DemonFruit = 1
  
  local function AutoStats()
    local function AddStats(Stats)
      if Player.Data.Points.Value >= 1 then
        local Points = math.clamp(PointsSlider, 1, Player.Data.Points.Value)
			  FireRemote("AddPoint", Stats, Points)
			end
    end
    
    while getgenv().AutoStats do task.wait()
      if Melee then
        AddStats("Melee")
      end
      if Defense then
        AddStats("Defense")
      end
      if Sword then
        AddStats("Sword")
      end
      if Gun then
        AddStats("Gun")
      end
      if DemonFruit then
        AddStats("Demon Fruit")
      end
    end
  end
  
  StatsTab:AddToggle({
    Name = "Auto Stats",
    Flag = "Stats/AutoStats",
    Callback = function(Value)
      getgenv().AutoStats = Value
      AutoStats()
    end
  })
  
  StatsTab:AddSlider({
    Name = "Select Points",
    Flag = "Stats/SelectPoints",
    Min = 1,
    Max = 100,
    Increase = 1,
    Default = 1,
    Callback = function(Value)
      PointsSlider = Value
    end
  })
  
  StatsTab:AddSection({"Select Stats"})
  
  StatsTab:AddToggle({Name = "Melee", Flag = "Stats/SelectMelee", Callback = function(Value)
    Melee = Value
  end})
  StatsTab:AddToggle({Name = "Defense", Flag = "Stats/SelectDefense", Callback = function(Value)
    Defense = Value
  end})
  StatsTab:AddToggle({Name = "Sword", Flag = "Stats/SelectSword", Callback = function(Value)
    Sword = Value
  end})
  StatsTab:AddToggle({Name = "Gun", Flag = "Stats/SelectGun", Callback = function(Value)
    Gun = Value
  end})
  StatsTab:AddToggle({Name = "Demon Fruit", Flag = "Stats/SelectDemonFruit", Callback = function(Value)
    DemonFruit = Value
  end})
end
local Teleport = Window:MakeTab({"Teleport", "Locate"})
local Visual = Window:MakeTab({"Visual", "User"})
local Shop = Window:MakeTab({"Shop", "ShoppingCart"})
local Misc = Window:MakeTab({"Misc", "Settings"})

MainFarm:AddDropdown({
  Name = "Farm Tool",
  Options = {"Melee", "Sword", "Blox Fruit"},
  Default = {"Melee"},
  Flag = "Main/FarmTool",
  Callback = function(Value)
    getgenv().FarmTool = Value
  end
})

if PlayerLevel.Value >= MaxLavel and Sea3 then
  MainFarm:AddToggle({
    Name = "Start Multi Farm < BETA >",
    Callback = function(Value)
      table.foreach(AFKOptions, function(_,Val)
        task.spawn(function()
          Val:Set(Value)
        end)
      end)
    end
  })
end

MainFarm:AddSection({"Farm"})

MainFarm:AddToggle({"Auto Farm Level", false, function(Value)
  getgenv().AutoFarm_Level = Value;AutoFarm_Level()
end, Description = "Lavel Farm"})

MainFarm:AddToggle({"Auto Farm Nearest", false, function(Value)
  getgenv().AutoFarmNearest = Value;AutoFarmNearest()
end})

if Sea3 then
  table.insert(AFKOptions, MainFarm:AddToggle({"Auto Pirates Sea", false, function(Value)
    getgenv().AutoPiratesSea = Value;AutoPiratesSea()
  end}))
elseif Sea2 then
  MainFarm:AddToggle({"Auto Factory", false, function(Value)
    getgenv().AutoFactory = Value;AutoFactory()
  end})
end

--[[MainFarm:AddSection({"Christmas"})

local TimeLabel = MainFarm:AddLabel({"Text", "Time for next gift : nil"})

task.spawn(function()
  local Counter = workspace:WaitForChild("Countdown", 9e9)
  local SurfaceGui = Counter:WaitForChild("SurfaceGui", 9e9)
  local TextLabel = SurfaceGui:WaitForChild("TextLabel", 9e9)
  
  while task.wait() do
    TimeLabel:SetTitle("Time to Next gift : " .. TextLabel.Text)
    
    if TextLabel.Text ~= "STARTING!" then
      local TimerL, Timer = TextLabel.Text:split(":"), 0
      if tonumber(TimerL[2]) >= 1 then
        Timer = tonumber(TimerL[2]) * 60
      end
      Timer = Timer + tonumber(TimerL[3])
      
      getgenv().TimeToGift = Timer
    else
      getgenv().TimeToGift = 0
    end
  end
end)

if PlayerLevel.Value >= MaxLavel and Sea3 then
  MainFarm:AddToggle({"Auto Farm Candy", false, function(Value)
    getgenv().AutoCandy = Value
    task.spawn(function()
      local Enemies1 = workspace:WaitForChild("Enemies", 9e9)
      local Enemies2 = ReplicatedStorage
      
      local function GetProxyNPC()
        local Distance = math.huge
        local NPC = nil
        local plrChar = Player and Player.Character and Player.Character.PrimaryPart
        
        for _,npc in pairs(Enemies1:GetChildren()) do
          if npc.Name == "Isle Champion" or npc.Name == "Sun-kissed Warrior" or npc.Name == "Island Boy" or npc.Name == "Isle Outlaw" then
            if plrChar and npc and npc:FindFirstChild("HumanoidRootPart") and (plrChar.Position - npc.HumanoidRootPart.Position).Magnitude <= Distance then
              Distance = (plrChar.Position - npc.HumanoidRootPart.Position).Magnitude
              NPC = npc
            end
          end
        end
        for _,npc in pairs(Enemies2:GetChildren()) do
          if npc.Name == "Isle Champion" or npc.Name == "Sun-kissed Warrior" or npc.Name == "Island Boy" or npc.Name == "Isle Outlaw" then
            if plrChar and npc and npc:FindFirstChild("HumanoidRootPart") and (plrChar.Position - npc.HumanoidRootPart.Position).Magnitude <= Distance then
              Distance = (plrChar.Position - npc.HumanoidRootPart.Position).Magnitude
              NPC = npc
            end
          end
        end
        return NPC
      end
      
      while getgenv().AutoCandy do task.wait()
        if Configure("Candy") then
        else
          local Enemie = GetProxyNPC()
          if Enemie then
            PlayerTP(Enemie.HumanoidRootPart.CFrame + getgenv().FarmPos)
            pcall(function()PlayerClick()ActiveHaki()EquipTool()BringNPC(Enemie, true)end)
          end
        end
      end
    end)
  end})
end

MainFarm:AddToggle({"Auto Gift", false, function(Value)
  getgenv().AutoGift = Value
  task.spawn(function()
    local function GetGift()
      for _,part in pairs(workspace["_WorldOrigin"]:GetChildren()) do
        if part.Name == "Present" then
          if part:FindFirstChild("Box") and part.Box:FindFirstChild("ProximityPrompt") then
            return part, part.Box.ProximityPrompt
          end
        end
      end
    end
    
    while getgenv().AutoGift do task.wait()
      local Gift, Prompt = GetGift()
      
      if Gift and Gift.PrimaryPart then
        PlayerTP(Gift.PrimaryPart.CFrame)
        if Prompt then
          fireproximityprompt(Prompt)
        end
      elseif getgenv().TimeToGift < 90 then
        if Sea3 then
          PlayerTP(CFrame.new(-1076, 14, -14437))
        elseif Sea2 then
          PlayerTP(CFrame.new(-5219, 15, 1532))
        elseif Sea1 then
          PlayerTP(CFrame.new(1007, 15, -3805))
        end
      end
    end
  end)
end})]]

if Sea3 then
  MainFarm:AddSection({"Bone"})
  
  table.insert(AFKOptions, MainFarm:AddToggle({
    Name = "Auto Farm Bone",
    Callback = function(Value)
      getgenv().AutoFarmBone = Value
      AutoFarmBone()
    end
  }))
  
  table.insert(AFKOptions, MainFarm:AddToggle({
    Name = "Auto Hallow Scythe",
    Callback = function(Value)
      getgenv().AutoSoulReaper = Value
      AutoSoulReaper()
    end
  }))
  
  table.insert(AFKOptions, MainFarm:AddToggle({
    Name = "Auto Trade Bone",
    Callback = function(Value)
      getgenv().AutoTradeBone = Value
      while getgenv().AutoTradeBone do task.wait()
        FireRemote("Bones", "Buy", 1, 1)
      end
    end
  }))
  
elseif Sea2 then
  
  MainFarm:AddSection({"Ectoplasm"})
  
  MainFarm:AddToggle({
    Name = "Auto Farm Ectoplasm",
    Callback = function(Value)
      getgenv().AutoFarmEctoplasm = Value
      AutoFarmEctoplasm()
    end
  })
end

MainFarm:AddSection({"Chest"})

MainFarm:AddToggle({
  Name = "Auto Chest < Tween >",
  Callback = function(Value)
    getgenv().AutoChestTween = Value
    AutoChestTween()
  end
})

MainFarm:AddToggle({
  Name = "Auto Chest < Bypass >",
  Callback = function(Value)
    getgenv().AutoChestBypass = Value
    AutoChestBypass()
  end
})

MainFarm:AddSection({"Bosses"})

MainFarm:AddButton({
  Name = "Update Boss List",
  Callback = function()
    pcall(function()UpdateBossList()end)
  end
})

local BossList = MainFarm:AddDropdown({
  Name = "Boss List",
  Callback = function(Value)
    getgenv().BossSelected = Value
  end
})

function UpdateBossList()
  local NewOptions = {}
  for ___,NameBoss in pairs(BossLi
