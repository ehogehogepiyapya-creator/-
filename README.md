print("Tsukuyomihub やおよろー！")

local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/jadpy/suki/refs/heads/main/orion"))()

-- ピンクグラデーションエフェクト（自動適用）
local function applyPinkGradientEffect()
    local CoreGui = game:GetService("CoreGui")
    local _Players2 = game:GetService("Players")
    local _time2 = 0
    local _strokes2 = {}
    local function _reg2(obj)
        if obj:IsA("UIStroke") then table.insert(_strokes2, obj) end
    end
    local function _scan2(gui)
        for _, v in ipairs(gui:GetDescendants()) do _reg2(v) end
        gui.DescendantAdded:Connect(_reg2)
    end
    local _plr2 = _Players2.LocalPlayer
    if _plr2 then
        local _pg2 = _plr2:WaitForChild("PlayerGui")
        pcall(_scan2, CoreGui)
        pcall(_scan2, _pg2)
    end
    local function _pinkColor(t)
        local c = {Color3.fromRGB(255,182,193), Color3.fromRGB(255,105,180), Color3.fromRGB(219,112,147)}
        local p = (math.sin(t*2)+1)/2
        if p < 0.5 then return c[1]:Lerp(c[2], p*2) else return c[2]:Lerp(c[3], (p-0.5)*2) end
    end
    game:GetService("RunService").RenderStepped:Connect(function(dt)
        _time2 = _time2 + dt*0.5
        for i = #_strokes2, 1, -1 do
            local s = _strokes2[i]
            if s and s.Parent then s.Color = _pinkColor(_time2 + i*0.01)
            else table.remove(_strokes2, i) end
        end
    end)
end
pcall(applyPinkGradientEffect)

local service = {
	Workspace = game:GetService("Workspace"),
	Players = game:GetService("Players"),
	ReplicatedStorage = game:GetService("ReplicatedStorage"),
	RunService = game:GetService("RunService"),
    VirtualUser = game:GetService("VirtualUser"),
    TeleportService = game:GetService("TeleportService"),
	SoundService = game:GetService("SoundService"),
    Debris = game:GetService("Debris"),
	Lighting = game:GetService("Lighting"),
	TweenService = game:GetService("TweenService"),
	UserInputService = game:GetService("UserInputService")
}

local CharacterEvents = service.ReplicatedStorage:WaitForChild("CharacterEvents")
local DataEvents = service.ReplicatedStorage:FindFirstChild("DataEvents")
local GrabEvents = service.ReplicatedStorage:WaitForChild("GrabEvents")
local PlayerEvents = service.ReplicatedStorage:WaitForChild("PlayerEvents")
local SlotTimeEvent = service.ReplicatedStorage:WaitForChild("SlotEvents")
local Struggle = CharacterEvents:WaitForChild("Struggle")
local RagdollRemote = CharacterEvents:WaitForChild("RagdollRemote")
local CreateGrabLine = GrabEvents:WaitForChild("CreateGrabLine")
local DestroyGrabLine = GrabEvents:WaitForChild("DestroyGrabLine")
local SetNetworkOwner = GrabEvents:WaitForChild("SetNetworkOwner")
local MenuToys = service.ReplicatedStorage:WaitForChild("MenuToys")
local SpawnToyRemoteFunction = MenuToys:WaitForChild("SpawnToyRemoteFunction")
local BuyToyRemoteFunction = MenuToys:WaitForChild("BuyToyRemoteFunction")
local DestroyToy = MenuToys:WaitForChild("DestroyToy")
local StickyPartEvent = PlayerEvents:WaitForChild("StickyPartEvent")
local SlotTime = SlotTimeEvent:WaitForChild("SlotTime")

local localPlayer = service.Players.LocalPlayer
local canSpawnToy = localPlayer:WaitForChild("CanSpawnToy")
local PlayerGui = localPlayer:WaitForChild("PlayerGui")
local PlayerScripts = localPlayer:WaitForChild("PlayerScripts")
local CharacterAndBeamMove = PlayerScripts:WaitForChild("CharacterAndBeamMove")
local Camera = service.Workspace.CurrentCamera
local Mouse = localPlayer:GetMouse()

local SpawnedInToys = service.Workspace:WaitForChild(localPlayer.Name .. "SpawnedInToys")
local mapHole = service.Workspace:WaitForChild("Map"):WaitForChild("Hole"):WaitForChild("PoisonBigHole")
local extPart = mapHole:FindFirstChild("ExtinguishPart")

local function GetCharacter()
    return localPlayer.Character or localPlayer.CharacterAdded:Wait()
end

local function PLCF()
    local c = GetCharacter()
    return c and c:FindFirstChild("HumanoidRootPart") and c.HumanoidRootPart.CFrame
end

local function HRP()
    local char = localPlayer.Character or localPlayer.CharacterAdded:Wait()
    return char:WaitForChild("HumanoidRootPart")
end

local function GetRoot(character)
    local char = character or localPlayer.Character
    return char and char:FindFirstChild("HumanoidRootPart")
end

local function getLocalHum()
    local char = localPlayer.Character
    return char and char:FindFirstChildOfClass("Humanoid")
end

local function getInv()
    return service.Workspace:FindFirstChild(localPlayer.Name .. "SpawnedInToys")
end

local function getCharInfo()
    local char = localPlayer.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    return char, hrp, hum
end

local function SPNTOY(name, CFE, vector)
    SpawnToyRemoteFunction:InvokeServer(
        name,
        CFE,
        vector or Vector3.zero
    )
    return SpawnedInToys:WaitForChild(name, 5)
end

local function destroyToy(model)
    DestroyToy:FireServer(model)
end

local function NBNotification(Message)
    OrionLib:MakeNotification({
        Name = "Tsukuyomihub やおよろー！",
        Content = Message,
        Image = "rbxassetid://16570630989",
        Time = 3
    })
end

local Config = {
    strengthConnection = nil,
    nageru = 400,
	killGrabToggle = false,
	DeathGrabToggle = nil,
	skyGrabToggle = false,
	Cons = {},
	RagdollGrab = false,
	spinGrabToggle = false,
	teleportUpToggle = false,
	teleportDownToggle = false,
	chaosGrabToggle = false,
	supersonicGrabToggle = false,
	freezeGrabToggle = false,
	noclipGrabToggle = false,
	anchorGrabToggle = false,
	masslessGrabToggle = false,

    FloatAmount = 16,
    selectedTarget = nil,
    ORHHi = nil,
    Running = false,
    ToyName = "CreatureBlobman",
    Range = 30,
    AutoBlobSit = false,
	blobSeat = nil,
	BlobmanESP = false,
	AIRNJIAD = {},
    RDESDG = nil,
    BLESP = false,
    BlobNoclip = false,
    BlobSpeedValue = 16,
    BlobSpeedToggle = false,

	AntiGrabToggle = false,
    ragdollCount = 0,
    antiGrabProce = false,
	Ragdoll = false,
    GEWgge = nil,
    SafePosition = nil,
    RestoreFrames = 0,
    GSEGESgggs = false,
    antibananaSit = false,
	antiStickyToggle = false,
	paintPartsBackup = {},
	paintConnections = {},
    SelectedSpawnLocation = "Spawn",
	AutoRespawnTP = false,
	airSuspendActive = false,
	airSuspendCoroutine = nil,
	SelectedCounterMode = "Death",
    antiKickCoroutine = nil,
	notifiedNoCoins = false,
    antikickV2 = false,
    antiAFKToggle = false,

    walkSpeedValue = 5,
    jumpPowerValue = 24,
    gravityPower = 100,
    defGravity = 90,
    fov = 70,
    deffov = 70,
    GravityToggle = false,
    jumpConnection = nil,
    walkSpeedToggle = false,
    infiniteJumpToggle = false,
    fovToggle = false,
    noclip = false,
    NoclipConnection = nil,
    freeze = nil,

    PlaceTP = "Green House",
    PlayerToTeleport = nil,
    PlayerToTeleportDirection = "Behind",
    PlayerMap = {},
    LoopPlayerTP = false,
    LockCameraOnPlayer = false,
    ViewCameraOnPlayer = false,
    TeleportPlayerOffset = 1,
    LoopTeleportToggle = false,
    CameraConnection = nil,

    joinNotify = false,
    leaveNotify = false,
    blobWebhookToggle = false,
    BlobConnections = {},
    BlobPlayerAddedConnection = nil,
    kickConnection = nil,

    auraRadius = 600,    
	auraCoroutine = nil,
    SitAuraToggle = false,
    SitauraConnection = nil,
    spinSpeed = 150,
    spinAuraToggle = false,
    spinConnection = nil,
    LaunchAuraCoroutine = nil,
    RagdollAuraToggle = false,
    deathAuraToggle = false,
    deathConnection = nil,
    VoidAura = false,
    NoclipAura = false,
    FlingAura = false,
	FlingRadius = 25,
    FlingTarget = 1,
    FlingPower = 200,
    WhitelistFriends = false,

    HeartToggle = false,
    Height = 5,
    Size = 8,
    ObjectCount = 20,
    RotationSpeed = 1,
    PulseSpeed = 2,
    PulseAmplitude = 0.5,
    FollowMode = "Follow Player",
    FrozenCFrame = nil,
    OrientationMode = "Default",
    TargetPlayer = localPlayer,
    HeartPoints = {},
    HeartLoopConn = nil,
    HeartTime = 0,

	BindEnabled = false,
    ZoomBind = false,
    ZFOV = 30,
    MIZ = 5,
    MXZ = 120,
    ZSP = 2,

    AutoSlot = false,
    savedPosition = nil,
    isTeleported = false,
	AutomationToggle = false,
	AutoConn = nil,
    ItemA = "InstrumentWoodwindOcarina",
    ItemB = "FoodMayonnaise",
    Seats = {},
    Conns = {},
    Riders = {},
    Time = 0,
    SelectedPlot = "Witch House",
	autoHouseTP = false,

    PlayerList = {},
    noclipConnection = nil,
    Noclipoff = false,
    SavedCFrame = nil,
    Active = false,
    Queue = {},
    BasePos = nil,
    SavedCamCFrame = nil,
    GrabALLitemT = false,
    drop = CFrame.new(- 238.98, - 256.01, - 123.97),
    ALLRadius = 25,
    Connection = nil,
    CamBlock = Instance.new("Part"),
	FireAllActive = false,
	IgnoreIsInPlot = false,
    WhitelistFriends2 = false,
    jerkOffActive = false,
    jerkOffAnimTrack = nil,

    MinDistance = 3,
    PcDistance = 0,
    LineDistance = 3,
    MaxExtendLine = 25,
    MinExtendLine = 3,
    IsTouching = false,
    FurtherExtend = false,
	InfLineToggle = false,
    SelectedPlayer = nil,
    IsRagServer = false,
    LineAmount = 100,
    PlayerNames = {},
    NameMap = {},
    ToggleState = false,
    hueOffset = 0,
    originalColors = nil,
    Lagserver = false,
    LagS = 150,

    BrightnessToggle = false,
    BrightnessLevel = service.Lighting.Brightness,
    FullBright = false,
    ShadowToggle = service.Lighting.GlobalShadows,
    ShadowLevel = service.Lighting.ShadowSoftness,
    FogToggle = false,
    FogLevel = service.Lighting.FogEnd
}

local function GrabParts(model)
    if model.Name ~= "GrabParts" then return end
    
    pcall(function()
        local grabPart = model:WaitForChild("GrabPart", 2)
        if not grabPart then return end
        
        local weld = grabPart:FindFirstChild("WeldConstraint")
        local targetPart = weld and weld.Part1
        if not targetPart then return end

        local targetCharacter = targetPart.Parent
        local targetHumanoid = targetCharacter:FindFirstChildOfClass("Humanoid")

        if Config.killGrabToggle and targetPart then
            targetCharacter:BreakJoints()
        end

        if Config.DeathGrabToggle and targetHumanoid then
            task.spawn(function()
                while targetCharacter.Parent and Config.DeathGrabToggle do
                    local targetPlayer = service.Players:GetPlayerFromCharacter(targetCharacter)

                    if CNWOSHIPOPl(targetPlayer) then
                        targetHumanoid.BreakJointsOnDeath = false
                        targetHumanoid:ChangeState(Enum.HumanoidStateType.Dead)
                        targetHumanoid.Jump = true
                        targetHumanoid.Sit = false

                        if targetHumanoid:GetStateEnabled(Enum.HumanoidStateType.Dead) then
                            DestroyGrabLine:FireServer(targetPart)
                        end
                    end
                    task.wait()
                end
            end)
        end

        if Config.skyGrabToggle or Config.supersonicGrabToggle then
            local speed = Config.supersonicGrabToggle and 1000 or 100
            local bv = Instance.new("BodyVelocity")
            bv.Velocity = Vector3.new(0, speed, 0)
            bv.MaxForce = Vector3.new(0, math.huge, 0)
            bv.Parent = grabPart
        end

        if Config.spinGrabToggle and targetPart then
            local bav = Instance.new("BodyAngularVelocity")
            bav.AngularVelocity = Vector3.new(0, 1000, 0)
            bav.MaxTorque = Vector3.new(0, math.huge, 0)
            bav.Parent = targetPart
        end

        if Config.teleportUpToggle then
            grabPart.Position = grabPart.Position - Vector3.new(0, 30, 0)
        end
        if Config.teleportDownToggle then
            grabPart.Position = grabPart.Position + Vector3.new(0, -30, 0)
        end
        if Config.chaosGrabToggle then
            local bp = Instance.new("BodyPosition", grabPart)
            bp.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
            local bg = Instance.new("BodyGyro", grabPart)
            bg.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)

            task.spawn(function()
                while Config.chaosGrabToggle and grabPart.Parent do
                    bp.Position = grabPart.Position + Vector3.new(math.random(-50,50), math.random(-50,50), math.random(-50,50))
                    bg.CFrame = CFrame.Angles(math.rad(math.random(-180,180)), math.rad(math.random(-180,180)), math.rad(math.random(-180,180)))
                    task.wait(0.1)
                end
                if bp then bp:Destroy() end
                if bg then bg:Destroy() end
            end)
        end

        if Config.freezeGrabToggle then
            local bp = Instance.new("BodyPosition", grabPart)
            bp.Position = grabPart.Position
            bp.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
        end

        if Config.noclipGrabToggle and not targetPart.Anchored then
            task.spawn(function()
                if targetCharacter:IsA("Model") then
                    local allParts = targetCharacter:GetDescendants()
                    local originalCollisionState = {}

                    for _, obj in pairs(allParts) do
                        if obj:IsA("BasePart") then
                            originalCollisionState[obj] = obj.CanCollide
                        end
                    end

                    while Config.noclipGrabToggle and grabPart.Parent do
                        for _, obj in pairs(allParts) do
                            if obj:IsA("BasePart") then
                                obj.CanCollide = false
                            end
                        end
                        task.wait(0.2)
                    end

                    for _, obj in pairs(allParts) do
                        if obj and obj:IsA("BasePart") and originalCollisionState[obj] ~= nil then
                            obj.CanCollide = originalCollisionState[obj]
                        end
                    end
                end
            end)
        end

        if Config.anchorGrabToggle and targetPart then
            local targetModel = targetPart:FindFirstAncestorOfClass("Model") or targetPart
            local partToAnchor = targetModel.PrimaryPart or targetPart

            local bp = Instance.new("BodyPosition", partToAnchor)
            bp.Position = partToAnchor.Position
            bp.MaxForce = Vector3.new(1e6, 1e6, 1e6)
            bp.P = 5000
            bp.D = 500

            local bg = Instance.new("BodyGyro", partToAnchor)
            bg.CFrame = partToAnchor.CFrame
            bg.MaxTorque = Vector3.new(1e6, 1e6, 1e6)
            bg.P = 5000
            bg.D = 500

            local highlight = Instance.new("Highlight", targetModel)
            highlight.Name = "AnchorHighlight"
            highlight.FillTransparency = 1
            highlight.OutlineTransparency = 0
            highlight.OutlineColor = Color3.fromRGB(0, 0, 255)
            highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop

            task.spawn(function()
                while Config.anchorGrabToggle and highlight.Parent do
                    service.TweenService:Create(highlight, TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {OutlineTransparency = 0.4}):Play()
                    task.wait(1)
                    if not highlight.Parent or not Config.anchorGrabToggle then break end
                    service.TweenService:Create(highlight, TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {OutlineTransparency = 0}):Play()
                    task.wait(1)
                end
                if highlight then highlight:Destroy() end
                if bp then bp:Destroy() end
                if bg then bg:Destroy() end
            end)
        end

        if Config.masslessGrabToggle then
            local dp = model:FindFirstChild("DragPart")
            if dp then
                local ap = dp:FindFirstChild("AlignPosition")
                local ao = dp:FindFirstChild("AlignOrientation")
                if ap then
                    ap.Responsiveness = 200
                    ap.MaxForce = math.huge
                    ap.MaxVelocity = math.huge
                end
                if ao then
                    ao.Responsiveness = 200
                    ao.MaxTorque = math.huge
                end
            end
        end

    end)
end
local function SAEX(char)
    local hum = char:WaitForChild("Humanoid", 5)
    local rd = hum and hum:WaitForChild("Ragdolled", 5)
    if rd and rd:IsA("BoolValue") then
        if Config.AntiExploConn then Config.AntiExploConn:Disconnect() end
        Config.AntiExploConn = rd:GetPropertyChangedSignal("Value"):Connect(function()
            for _, p in ipairs(char:GetChildren()) do
                if p:IsA("BasePart") then p.Anchored = rd.Value end
            end
        end)
    end
end

local function getBlobman()
    local inv = getInv()
    local v = inv and inv:FindFirstChild("CreatureBlobman")
    if not v then
        for _, p in ipairs(service.Workspace.PlotItems:GetChildren()) do
            local m = p:FindFirstChild("CreatureBlobman")
            if m and m:FindFirstChild("PlayerValue") and m.PlayerValue.Value == localPlayer.Name then
                v = m
                break
            end
        end
    end
    return v
end

local function spawnBlobman()
    SpawnToyRemoteFunction:InvokeServer("CreatureBlobman", GetRoot().CFrame, Vector3.zero)
    task.wait(1.0)
    return getBlobman()
end

local function resetBlobmanPhysics()
    local blob = getBlobman()
    if not blob then return end
    for _, part in blob:GetDescendants() do
        if part:IsA("BasePart") then
            part.AssemblyLinearVelocity = Vector3.zero
            part.AssemblyAngularVelocity = Vector3.zero
        end
    end
end

local function stabilizeBlobman()
    local blob = getBlobman()
    local root = GetRoot()
    if not blob or not blob:FindFirstChild("VehicleSeat") or not root then return end
    local seat = blob.VehicleSeat
    seat.CFrame = seat.CFrame + (root.CFrame.Position - seat.Position) * 0.1
end

local function isSittingOnBlobman()
    local hum = getLocalHum()
    if not hum then return false end
    local blob = getBlobman()
    if not blob or not blob:FindFirstChild("VehicleSeat") then return false end
    return hum.Sit and hum.SeatPart == blob.VehicleSeat
end

local function ensureSitBlobman()
    local blob = getBlobman()
    if not blob or not blob:FindFirstChild("VehicleSeat") then return false end
    local seat = blob.VehicleSeat
    local hum = getLocalHum()
    if hum and not hum.Sit then
        seat:Sit(hum)
        task.wait(0.6)
    end
    resetBlobmanPhysics()
    stabilizeBlobman()
    return isSittingOnBlobman()
end

local function blobGrab(blob, target, side)
    if not blob then return end
    local detector = blob:FindFirstChild(side .. "Detector")
    if not detector then return end
    local weld = detector:FindFirstChild(side .. "Weld")
    if not weld then return end
    local script = blob:FindFirstChild("BlobmanSeatAndOwnerScript", true)
    if not script then return end
    local remote = script:FindFirstChild("CreatureGrab")
    if remote then
        pcall(function() remote:FireServer(detector, target, weld) end)
    end
end

local function blobDrop(blob, target, side)
    if not blob then return end
    local detector = blob:FindFirstChild(side .. "Detector")
    if not detector then return end
    local script = blob:FindFirstChild("BlobmanSeatAndOwnerScript", true)
    if not script then return end
    local remote = script:FindFirstChild("CreatureDrop")
    if remote then
        pcall(function() remote:FireServer(detector, target) end)
    end
end

local function ensureBlob()
    local blob = getBlobman()
    if not blob then blob = spawnBlobman() end
    local hum = getLocalHum()
    if hum and not hum.Sit then
        blob.VehicleSeat:Sit(hum)
        task.wait(0.22)
    end
    return getBlobman()
end

local function getPlayerList()
    local list = {}
    for _, plr in pairs(service.Players:GetPlayers()) do
        if plr ~= localPlayer then
            table.insert(list, string.format("%s (ID: %s)", plr.DisplayName, plr.Name))
        end
    end
    return list
end

local function getPlayerFromSelection(selection)
    if not selection then return nil end
    for _, plr in pairs(service.Players:GetPlayers()) do
        local format = string.format("%s (ID: %s)", plr.DisplayName, plr.Name)
        if format == selection then return plr end
    end
    return nil
end

local function checkTarget()
    local target = getPlayerFromSelection(Config.selectedTarget)
    if not target then
        OrionLib:MakeNotification({Name = "エラー", Content = "ターゲットが見つかりません", Time = 3})
        return false
    end
    return target
end

local function spawnBlobmanAtPlayer()
    local char = localPlayer.Character
    if not char then return end

    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end

    local pos = root.CFrame + Vector3.new(0, 3, 0)

    return SpawnToyRemoteFunction:InvokeServer(
        "CreatureBlobman",
        pos,
        Vector3.new(0, 59.667, 0)
    )
end

service.RunService.Heartbeat:Connect(function()
    if Config.AutoBlobSit then
        local char = localPlayer.Character
        if not char then return end

        local humanoid = char:FindFirstChild("Humanoid")
        if not humanoid then return end

        currentBlobman = getBlobman()

        if not currentBlobman then
            spawnBlobmanAtPlayer()
            currentBlobman = getBlobman()
        end

        if currentBlobman then
            local seat = currentBlobman:FindFirstChild("VehicleSeat")
            if seat and humanoid.SeatPart ~= seat then
                seat:Sit(humanoid)
                Config.blobSeat = seat
            end
        end
    end
end)

if not extPart then
    extPart = Instance.new("Part")
    extPart.Name = "ExtinguishPart"
    extPart.Size = Vector3.new(4, 1, 4)
    extPart.Anchored = true
    extPart.CanCollide = false
    extPart.Transparency = 1
    extPart.Position = mapHole.Position
    extPart.Parent = mapHole
end

local originalPos = extPart.Position

local SpawnLocations = {
    ["Green Safe-House"] = CFrame.new(-584, -6, 93),
    ["Chinese Safe-House"] = CFrame.new(579, 124, -94),
    ["Spawn"] = CFrame.new(4, -7, -3),
    ["Blue Safe-House"] = CFrame.new(538, 96, -372),
    ["Witch Safe-House"] = CFrame.new(296, -4, 494),
    ["Red Safe-House"] = CFrame.new(-516, -6, -162)
}

local placeLocations = {
    ["Green Safe-House"] = CFrame.new(-584, -6, 93),
    ["Chinese Safe-House"] = CFrame.new(579, 124, -94),
    ["Farm House"] = CFrame.new(-234, 83, -324),
    ["Spawn"] = CFrame.new(4, -7, -3),
    ["Blue Safe-House"] = CFrame.new(538, 96, -372),
    ["Secret Big Cave"] = CFrame.new(17, -7, 539),
    ["Secret Train Cave"] = CFrame.new(500, 62, -307),
    ["Mine Cave"] = CFrame.new(-254, -7, 518),
    ["Witch Safe-House"] = CFrame.new(296, -4, 494),
    ["Red Safe-House"] = CFrame.new(-516, -6, -162)
}

local gui = Instance.new("ScreenGui")
gui.Name = "ExtendControlGui"
gui.ResetOnSpawn = false
gui.Parent = localPlayer:WaitForChild("PlayerGui")

gui.Enabled = service.UserInputService.TouchEnabled

local CAG = localPlayer.PlayerGui:FindFirstChild("ContextActionGui")
if CAG then
    CAG.DescendantAdded:Connect(function(descendant)
        if Config.InfLineToggle and descendant:IsA("ImageButton") then
            local actionIcon = descendant:WaitForChild("ActionIcon", 2)
            if actionIcon and (actionIcon.Image == "rbxassetid://9603826756" or actionIcon.Image == "rbxassetid://9603831913") then
                actionIcon.Parent.Visible = false
            end
        end
    end)
end

local imageButton = Instance.new("ImageButton")
imageButton.Size = UDim2.new(0, 45, 0, 45)
imageButton.Position = UDim2.new(1, -70, 1, -259)
imageButton.Image = "rbxassetid://97166444"
imageButton.BackgroundTransparency = 1
imageButton.ImageTransparency = 0.2
imageButton.ImageColor3 = Color3.fromRGB(142, 142, 142)
imageButton.Visible = false
imageButton.Parent = gui

local imageLabel = Instance.new("ImageLabel")
imageLabel.Size = UDim2.new(1, 0, 1, 0)
imageLabel.Image = "rbxassetid://9603831913"
imageLabel.BackgroundTransparency = 1
imageLabel.Parent = imageButton

local imageButtonDe = Instance.new("ImageButton")
imageButtonDe.Size = UDim2.new(0, 45, 0, 45)
imageButtonDe.Position = UDim2.new(1, -70, 1, -211)
imageButtonDe.Image = "rbxassetid://97166444"
imageButtonDe.BackgroundTransparency = 1
imageButtonDe.ImageTransparency = 0.2
imageButtonDe.ImageColor3 = Color3.fromRGB(142, 142, 142)
imageButtonDe.Visible = false
imageButtonDe.Parent = gui

local imageLabelDe = Instance.new("ImageLabel")
imageLabelDe.Size = UDim2.new(1, 0, 1, 0)
imageLabelDe.Image = "rbxassetid://9603826756"
imageLabelDe.BackgroundTransparency = 1
imageLabelDe.Parent = imageButtonDe

local function GetPlayerCharacter()
    local char = localPlayer.Character
    if char and char:FindFirstChild("HumanoidRootPart") and char:FindFirstChildOfClass("Humanoid") then
        return char
    end
end

local function toggleButtonState(state)
    local isVisible = state and Config.InfLineToggle
    imageButton.Visible = isVisible
    imageButton.Active = isVisible
    imageButtonDe.Visible = isVisible
    imageButtonDe.Active = isVisible
end

local function adjustDistance(amount)
    if not Config.InfLineToggle then return end

    Config.PcDistance = Config.PcDistance + amount
    if Config.PcDistance < Config.MinDistance then
        Config.PcDistance = Config.MinDistance
    end

    if senv and senv.distance then
        senv.distance = Config.PcDistance
    end
end

Workspace.ChildAdded:Connect(function(child)
    if child.Name == "GrabParts" and child:IsA("Model") then
        if Config.InfLineToggle and (service.UserInputService.MouseEnabled or service.UserInputService.TouchEnabled) then
            local grabPart = child:WaitForChild("GrabPart")
            local dragPart = child:WaitForChild("DragPart")
            
            local bodyPos = Instance.new("BodyPosition")
            bodyPos.MaxForce = Vector3.new(275000, 275000, 275000)
            bodyPos.P = 20000
            bodyPos.D = 950
            bodyPos.Position = grabPart.Position
            bodyPos.Parent = grabPart
            
            Config.PcDistance = (grabPart.Position - service.Workspace.CurrentCamera.CFrame.Position).Magnitude
            dragPart:WaitForChild("AlignPosition").Enabled = false
            
            task.spawn(function()
                while child.Parent do
                    bodyPos.Position = service.Workspace.CurrentCamera.CFrame.Position + (service.Workspace.CurrentCamera.CFrame.LookVector * Config.PcDistance)
                    task.wait()
                end
                Config.PcDistance = 0
                bodyPos:Destroy()
            end)
        end
        toggleButtonState(true)
    end
end)

service.Workspace.ChildRemoved:Connect(function(child)
    if child.Name == "GrabParts" and child:IsA("Model") then
        toggleButtonState(false)
    end
end)

local function startLongPress(mode)
    while Config.IsTouching do
        if mode == "increase" then
            adjustDistance(Config.LineDistance)
        else
            adjustDistance(-Config.LineDistance)
        end
        task.wait(0.1)
    end
end

imageButton.InputBegan:Connect(function(input, processed)
    if not processed and input.UserInputType == Enum.UserInputType.Touch then
        Config.IsTouching = true
        task.spawn(startLongPress, "increase")
    end
end)

imageButtonDe.InputBegan:Connect(function(input, processed)
    if not processed and input.UserInputType == Enum.UserInputType.Touch then
        Config.IsTouching = true
        task.spawn(startLongPress, "decrease")
    end
end)

local function endTouch(input)
    if input.UserInputType == Enum.UserInputType.Touch then
        Config.IsTouching = false
    end
end

imageButton.InputEnded:Connect(endTouch)
imageButtonDe.InputEnded:Connect(endTouch)

service.UserInputService.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseWheel then
        if Config.PcDistance < 11 then Config.PcDistance = 11 end
        local direction = input.Position.Z > 0 and 1 or -1
        adjustDistance(direction * Config.LineDistance)
    end
end)

local function GetTableKeys(tbl)
    local keys = {}
    for k, _ in pairs(tbl) do
        table.insert(keys, k)
    end
    return keys
end

local function GetPlayerRoot()
    local char = localPlayer.Character
    if char then
        return char:FindFirstChild("HumanoidRootPart")
    end
end

local function TeleportPlayer(cf)
    if not cf then return end
    local char = localPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if hrp then
        hrp.CFrame = cf
    end
end

local function GetPlayerList()
    local list = {}
    Config.PlayerMap = {}

    for _, player in pairs(service.Players:GetPlayers()) do
        if player ~= localPlayer then
            local formatted = player.DisplayName .. " (ID: " .. player.Name .. ")"

            table.insert(list, formatted)
            Config.PlayerMap[formatted] = player.Name
        end
    end

    return list
end

local function TeleportWithOffset(targetCFrame, myRoot, targetCharacter, targetName)
    local offset = Config.TeleportPlayerOffset + 1
    local resultCFrame

    if Config.PlayerToTeleportDirection == "Behind" then
        resultCFrame = CFrame.new(targetCFrame.Position - targetCFrame.LookVector * offset)
    elseif Config.PlayerToTeleportDirection == "Front" then
        resultCFrame = CFrame.new(targetCFrame.Position + targetCFrame.LookVector * offset)
    elseif Config.PlayerToTeleportDirection == "Right" then
        resultCFrame = CFrame.new(targetCFrame.Position + targetCFrame.RightVector * offset)
    elseif Config.PlayerToTeleportDirection == "Left" then
        resultCFrame = CFrame.new(targetCFrame.Position - targetCFrame.RightVector * offset)
    end

    if resultCFrame then
        TeleportPlayer(resultCFrame)
    end
end



service.Players.PlayerAdded:Connect(function(player)
    if not Config.joinNotify then return end
    local message = player.DisplayName .. " (" .. player.Name .. ") がゲームに参加しました。"
    NBNotification(message)
end)

service.Players.PlayerRemoving:Connect(function(player)
    if not Config.leaveNotify then return end
    local message = player.DisplayName .. " (" .. player.Name .. ") がゲームを退出しました。"
    NBNotification(message)
end)


local function CanAuraTarget(targetPlayer)
    if not targetPlayer or targetPlayer == localPlayer then return false end
    if not targetPlayer.Character then return false end

    if Config.WhitelistFriends and localPlayer:IsFriendsWith(targetPlayer.UserId) then
        return false
    end

    if table.find(kickAuraWhitelist or {}, targetPlayer.Name) then
        return false
    end

    local hum = targetPlayer.Character:FindFirstChildOfClass("Humanoid")
    return hum and hum.Health > 0
end

local function onSitaura()
    Config.SitauraConnection = service.RunService.Heartbeat:Connect(function()
        if not Config.SitAuraToggle then return end
        local rootPart = GetPlayerCharacter() and GetPlayerCharacter().HumanoidRootPart
        if not rootPart then return end

        for _, player in ipairs(service.Players:GetPlayers()) do
            if CanAuraTarget(player) then
                local torso = player.Character:FindFirstChild("Torso") or player.Character:FindFirstChild("UpperTorso")
                local humanoid = player.Character:FindFirstChildOfClass("Humanoid")

                if torso and humanoid and (torso.Position - rootPart.Position).Magnitude <= Config.auraRadius then
                    SetNetworkOwner:FireServer(torso, rootPart.CFrame)
                    humanoid.Sit = true
                end
            end
        end
    end)
end

local function offSitAura()
	if Config.SitauraConnection then
		Config.SitauraConnection:Disconnect()
		Config.SitauraConnection = nil
	end
end

local function onSpinAura()
    if Config.spinConnection then return end
    Config.spinAuraToggle = true

    Config.spinConnection = task.spawn(function()
        while Config.spinAuraToggle do
            local rootPart = GetPlayerCharacter() and GetPlayerCharacter().HumanoidRootPart
            if rootPart then
                for _, player in ipairs(service.Players:GetPlayers()) do
                    if CanAuraTarget(player) then
                        local torso = player.Character:FindFirstChild("UpperTorso") or player.Character:FindFirstChild("Torso") or player.Character:FindFirstChild("HumanoidRootPart")
                        if torso and (torso.Position - rootPart.Position).Magnitude <= Config.auraRadius then
                            SetNetworkOwner:FireServer(torso, rootPart.CFrame)
                            local spin = torso:FindFirstChild("SpinForce") or Instance.new("BodyAngularVelocity")
                            spin.Name = "SpinForce"
                            spin.MaxTorque = Vector3.new(999999, 999999, 999999)
                            spin.AngularVelocity = Vector3.new(0, Config.spinSpeed, 0)
                            spin.Parent = torso
                        end
                    end
                end
            end
            task.wait(0.05)
        end
    end)
end

local function offSpinAura()

	Config.spinAuraToggle = false

	for _, player in ipairs(service.Players:GetPlayers()) do
		if player.Character then
			local torso = player.Character:FindFirstChild("UpperTorso")
				or player.Character:FindFirstChild("Torso")
				or player.Character:FindFirstChild("HumanoidRootPart")

			if torso then
				local spin = torso:FindFirstChild("SpinForce")
				if spin then
					spin:Destroy()
				end
			end
		end
	end

	Config.spinConnection = nil
end


function onAirSuspendAura()
    Config.LaunchAuraCoroutine = coroutine.create(function()
        while true do
            pcall(function()
                local root = GetPlayerCharacter() and GetPlayerCharacter().HumanoidRootPart
                if not root then return end

                for _, player in pairs(service.Players:GetPlayers()) do
                    if CanAuraTarget(player) then
                        local torso = player.Character:FindFirstChild("Torso")
                        if torso and (torso.Position - root.Position).Magnitude <= Config.auraRadius then
                            SetNetworkOwner:FireServer(torso, player.Character.HumanoidRootPart.FirePlayerPart.CFrame)
                            task.wait(0.1)
                            local bodyVel = torso:FindFirstChild("LaunchForce") or Instance.new("BodyVelocity", torso)
                            bodyVel.Name = "LaunchForce"
                            bodyVel.Velocity = Vector3.new(0, 200000000000, 0)
                            bodyVel.MaxForce = Vector3.new(0, math.huge, 0)
                            service.Debris:AddItem(bodyVel, 1)
                        end
                    end
                end
            end)
            task.wait(0.02)
        end
    end)
    coroutine.resume(Config.LaunchAuraCoroutine)
end


function offAirSuspendAura()
    if Config.LaunchAuraCoroutine then
        coroutine.close(Config.LaunchAuraCoroutine)
        Config.LaunchAuraCoroutine = nil
    end
end

task.spawn(function()
    while true do
        if Config.RagdollAuraToggle then
            local myRoot = GetPlayerCharacter() and GetPlayerCharacter().HumanoidRootPart
            if myRoot then
                for _, player in ipairs(service.Players:GetPlayers()) do
                    if CanAuraTarget(player) then
                        local targetRoot = player.Character:FindFirstChild("HumanoidRootPart")
                        if targetRoot and (targetRoot.Position - myRoot.Position).Magnitude <= Config.auraRadius then
                            SetNetworkOwner:FireServer(targetRoot, targetRoot.CFrame)
                            targetRoot.AssemblyLinearVelocity = Vector3.new(0, -256, 0)
                        end
                    end
                end
            end
        end
        task.wait(0.15)
    end
end)

local function onHellSendAura()
    Config.gravityCoroutine = coroutine.create(function()
        while true do
            pcall(function()
                local rootPart = GetPlayerCharacter() and GetPlayerCharacter().HumanoidRootPart
                if not rootPart then return end
                local camera = workspace.CurrentCamera

                for _, player in ipairs(service.Players:GetPlayers()) do
                    if CanAuraTarget(player) then
                        local torso = player.Character:FindFirstChild("Torso")
                        if torso and (torso.Position - rootPart.Position).Magnitude <= Config.auraRadius then
                            SetNetworkOwner:FireServer(torso, rootPart.CFrame)
                            for _, part in ipairs(player.Character:GetDescendants()) do
                                if part:IsA("BasePart") then part.CanCollide = false end
                            end
                            local bodyPos = torso:FindFirstChild("HellAuraPos") or Instance.new("BodyPosition", torso)
                            bodyPos.Name = "HellAuraPos"
                            bodyPos.MaxForce = Vector3.new(1e5, 1e5, 1e5)
                            bodyPos.Position = rootPart.Position + (camera.CFrame.LookVector * 15) + Vector3.new(0, 5, 0)
                        end
                    end
                end
            end)
            task.wait(0.05)
        end
    end)
    coroutine.resume(Config.gravityCoroutine)
end

local function offHellSendAura()
	if Config.gravityCoroutine then
		coroutine.close(Config.gravityCoroutine)
		Config.gravityCoroutine = nil
	end
end

local function DeathAura(toggle)
    if Config.deathConnection then Config.deathConnection:Disconnect() end
    Config.deathAuraToggle = toggle
    if not toggle then return end

    Config.deathConnection = service.RunService.Heartbeat:Connect(function()
        for _, plr in ipairs(service.Players:GetPlayers()) do
            if CanAuraTarget(plr) then
                local char = plr.Character
                local hrp = char:FindFirstChild("HumanoidRootPart")
                local myHRP = GetPlayerCharacter() and GetPlayerCharacter().HumanoidRootPart
                if hrp and myHRP and (hrp.Position - myHRP.Position).Magnitude <= 25 then
                    pcall(function()
                        SetNetworkOwner:FireServer(hrp, hrp.CFrame)
                        task.wait(0.1)
                        DestroyGrabLine:FireServer(hrp)
                        for _, bp in ipairs(char:GetChildren()) do
                            if bp:IsA("BasePart") then bp.CFrame = CFrame.new(-1e9, 1e9, -1e9) end
                        end
                        local hum = char:FindFirstChildOfClass("Humanoid")
                        hum:ChangeState(Enum.HumanoidStateType.Dead)
                    end)
                end
            end
        end
    end)
end
local function IsPlayerCharacter(part)
    local model = part:FindFirstAncestorOfClass("Model")
    if model and service.Players:GetPlayerFromCharacter(model) then
        return true
    end
    return false
end

local function LookAt(fromPos, toPos)
    return CFrame.lookAt(fromPos, toPos)
end

local function ROS(part, ignoreDistance)
    if not part or not part:IsA("BasePart") then return end

    local character = GetCharacter()
    if not character then return end

    local root = character.HumanoidRootPart
    local distance = (root.Position - part.Position).Magnitude

    if ignoreDistance or distance <= 30 then
        pcall(function()
            SetNetworkOwner:FireServer(
                part,
                LookAt(root.Position, part.Position)
            )
        end)
    end
end

local function AttractionAura()
    task.spawn(function()
        while _G.AttractionAura do
            local myRoot = GetPlayerCharacter and GetPlayerCharacter:FindFirstChild("HumanoidRootPart")
            
            if not myRoot then
                task.wait(0.5)
                continue
            end

            for _, player in pairs(service.Players:GetPlayers()) do
                if CanAuraTarget(player) then 
                    local char = player.Character
                    local root = char:FindFirstChild("HumanoidRootPart")
                    local humanoid = char:FindFirstChildOfClass("Humanoid")

                    if root and humanoid then
                        ROS(root)

                        humanoid.Sit = false
                        humanoid:MoveTo(myRoot.Position)
                    end
                end
            end

            task.wait(0.1)
        end
    end)
end

local function runNoclipAura()
    task.spawn(function()
        while true do
            if Config.NoclipAura then
                local root = GetRoot(localPlayer.Character)
                if root then
                    for _, player in ipairs(service.Players:GetPlayers()) do
                        if CanAuraTarget(player) then
                            local targetRoot = player.Character:FindFirstChild("HumanoidRootPart")
                            
                            if targetRoot and (targetRoot.Position - root.Position).Magnitude <= Config.auraRadius then
                                ROS(targetRoot, targetRoot.CFrame)

                                for _, part in ipairs(player.Character:GetDescendants()) do
                                    if part:IsA("BasePart") then
                                        part.CanCollide = false
                                    end
                                end
                            end
                        end
                    end
                end
            end
            task.wait(0.5)
        end
    end)
end

runNoclipAura()

local function FlingPart(part)
    if not part then return end
    if not part:IsA("BasePart") then return end
    if part.Anchored then return end
    if part:FindFirstChild("FlingForce") then return end

    local myRoot = GetRoot(localPlayer.Character)
    if not myRoot then return end

    ROS(part, true)

    local direction = (part.Position - myRoot.Position).Unit

    local velocity = Instance.new("BodyVelocity")
    velocity.Name = "FlingForce"
    velocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    velocity.Velocity = Vector3.new(direction.X, 0.4, direction.Z) * Config.FlingPower
    velocity.Parent = part

    service.Debris:AddItem(velocity, 0.15)
end

local function StartFling()
    task.spawn(function()
        while Config.FlingAura do
            local myRoot = GetPlayerCharacter() and GetPlayerCharacter().HumanoidRootPart
            if myRoot then
                if Config.FlingTarget == 1 or Config.FlingTarget == 3 then
                    for _, player in pairs(service.Players:GetPlayers()) do
                        if CanAuraTarget(player) then
                            local root = player.Character:FindFirstChild("HumanoidRootPart")
                            if root and (root.Position - myRoot.Position).Magnitude <= Config.FlingRadius then
                                FlingPart(root)
                            end
                        end
                    end
                end

                if Config.FlingTarget == 2 or Config.FlingTarget == 3 then
                    local overlappingParts = workspace:GetPartBoundsInRadius(myRoot.Position, Config.FlingRadius)
                    
                    for _, part in pairs(overlappingParts) do
                        if part:IsA("BasePart") 
                           and not part.Anchored
                           and not part:IsDescendantOf(localPlayer.Character)
                           and not IsPlayerCharacter(part)
                        then
                            FlingPart(part)
                        end
                    end
                end
            end
            task.wait(0.12)
        end
    end)
end
SlotTime.OnClientEvent:Connect(function(remainingTime)
    Config.Time = remainingTime
    
    if Config.Time == 0 and Config.AutoSlot then
        task.spawn(function()
            local root = GetRoot()
            if not root then return end
            
            local originalPos = root.CFrame
            
            while Config.Time == 0 and Config.AutoSlot do
                for _, slot in ipairs(service.Workspace.Slots:GetChildren()) do
                    if not Config.AutoSlot then break end
                    
                    local handle = slot:FindFirstChild("SlotHandle") and slot.SlotHandle:FindFirstChild("Handle")
                    if handle then
                        task.wait(0.1)
                        root.CFrame = handle.CFrame
                        task.wait(0.2)
                        SetNetworkOwner:FireServer(handle, root.CFrame)
                    end
                end
                task.wait()
            end
            
            task.wait(1.5)
            root.CFrame = originalPos
        end)
    end
end)

local function TeleportHouse(cf)
    local char = GetCharacter()
    if char and typeof(cf) == "CFrame" then
        char.HumanoidRootPart.CFrame = cf
    end
end

local function GrabPart(part)
    local char = GetCharacter()
    if not char then return end
    if not part or not part:IsA("BasePart") then return end

    if localPlayer:DistanceFromCharacter(part.Position) <= 30 then
        SetNetworkOwner:FireServer(
            part,
            LookAt(
                char.HumanoidRootPart.Position,
                part.Position
            )
        )
    end
end

local PlotMap = {
    ["Common House"]   = "Plot1",
    ["Lumber House"]   = "Plot2",
    ["Witch House"]    = "Plot3",
    ["American House"] = "Plot4",
    ["Chinese House"]  = "Plot5"
}

local function GetPlot()
    local plots = service.Workspace:FindFirstChild("Plots")
    if not plots then return end

    local folder = PlotMap[Config.SelectedPlot]
    if folder then
        return plots:FindFirstChild(folder)
    end
end

local function PlotOwner()
    local plot = GetPlot()
    if not plot then return nil end

    local sign = plot:FindFirstChild("PlotSign")
    if not sign then return nil end

    local ownersFolder = sign:FindFirstChild("ThisPlotsOwners")
    if not ownersFolder then return nil end

    local valueObject = ownersFolder:FindFirstChild("Value")
    if valueObject and valueObject:IsA("StringValue") then
        return valueObject.Value
    end

    return nil
end

local function IsPlotOwned()
    local ownerName = PlotOwner()
    return ownerName and ownerName ~= ""
end

local function ClaimPlot()
    local plot = GetPlot()
    if not plot then return end
    if IsPlotOwned() then return end

    local sign = plot:FindFirstChild("PlotSign")
    if not sign then return end

    for _, child in pairs(sign:GetChildren()) do
        if child.Name == "Sign" then
            local grab = child:FindFirstChild("Plus")
                and child.Plus:FindFirstChild("PlusGrabPart")

            if grab then
                TeleportHouse(grab.CFrame * CFrame.new(-5,0,-5))

                for i = 1, 15 do
                    GrabPart(grab)
                    task.wait()
                end
            end
        end
    end
end

local function whiteListToggle(targetPlayer)
    if Config.WhitelistFriends2 then
        return localPlayer:IsFriendsWith(targetPlayer.UserId)
    end
    return false
end

function CSV(part)
    if not part:FindFirstChild("SkyVelocity") then
        local velocity = Instance.new("BodyVelocity", part)
        velocity.Name = "SkyVelocity"
        velocity.Velocity = Vector3.new(0, 1e14, 0)
        velocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    end
end

function TPPlayer(targetCFrame)
    local char = GetCharacter()
    if char and typeof(targetCFrame) == "CFrame" then
        local root = char.HumanoidRootPart
        local hum = char:FindFirstChildOfClass("Humanoid")
        root.CFrame = root.CFrame.Rotation + targetCFrame.Position
        if hum.SeatPart == nil or tostring(hum.SeatPart.Parent) ~= "CreatureBlobman" then
            hum.Sit = false
        end
    end
end

local function NoclipON()
    if not Config.noclipConnection then
        Config.Noclipoff = false
        Config.noclipConnection = service.RunService.Heartbeat:Connect(function()
            if not Config.Noclipoff and localPlayer.Character then
                for _, part in pairs(localPlayer.Character:GetChildren()) do
                    if part:IsA("BasePart") and part.CanCollide and part.Name ~= (floatName or "") then
                        part.CanCollide = false
                    end
                end
            end
        end)
    end
end

local function NoclipOFF()
    if not _G.NoclipToggle then
        if Config.noclipConnection then
            Config.noclipConnection:Disconnect()
            Config.noclipConnection = nil
        end
        Config.Noclipoff = true
    end
end

function CNWOSHIPOPl(targetPlayer)
    if typeof(targetPlayer) == "Instance" and targetPlayer:IsA("Player") and targetPlayer.Character then
        local head = targetPlayer.Character:FindFirstChild("Head")
        local ownerValue = head and head:FindFirstChild("PartOwner")
        if ownerValue and ownerValue.Value == localPlayer.Name then
            return ownerValue
        end
    end
    return false
end

local function IsValidTarget(target)
    if typeof(target) == "Instance" and target ~= localPlayer and target.Character then
        local char = target.Character
        if char:IsDescendantOf(service.Workspace) and char:FindFirstChild("HumanoidRootPart") then
            local hum = char:FindFirstChildOfClass("Humanoid")
            if hum and hum.Health > 0 then
                return true
            end
        end
    end
    return false
end

local function CanAttack(target)
    if IsValidTarget(target) then
        if Config.WhitelistFriends2 and whiteListToggle(target) then
            return false
        end

        local inPlot = target:FindFirstChild("InPlot")
        if not (inPlot and inPlot.Value) then
            return true
        end
    end
    return false
end


local function GetPlayerListForDropdown()
    local list = {}
    for _, v in pairs(game:GetService("Players"):GetPlayers()) do
        if v ~= game:GetService("Players").LocalPlayer then
            table.insert(list, string.format("%s (ID: %s)", v.DisplayName, v.Name))
        end
    end
    return list
end

local PlayerDropdown

local function RefreshPlayer()
    Config.PlayerNames = {}
    Config.NameMap = {}
    for _, localPlayer in ipairs(service.Players:GetPlayers()) do
        local display = localPlayer.DisplayName .. " (ID: " .. localPlayer.Name .. ")"
        table.insert(Config.PlayerNames, display)
        Config.NameMap[display] = localPlayer.Name
    end
    if PlayerDropdown then
        PlayerDropdown:Refresh(Config.PlayerNames, true)
    end
end

RefreshPlayer()

-- ====================================================================
-- さくらhub 統合ブロック
-- ====================================================================
local LocalPlayer = localPlayer
local Players     = service.Players
local RunService  = service.RunService
local UserInputService = game:GetService("UserInputService")

local Theme = { SliderColor = Color3.fromRGB(255, 105, 180) }

-- オブジェクトID設定
local ObjectIDConfig = {
    CurrentObjectID = "FireworkSparkler",
    AvailableObjects = {
        "FireworkSparkler","PalletLightBrown","GlassBoxGray","MusicKeyboard",
        "SpookyCandle1","Tetracube1","NinjaKatana","YouDecoy","BombMissile",
        "LadderLightBrown","CreatureBlobman","DiscoColorBall","MineralCrystalPink",
        "FloatingIsland","FlyingToyUfo","JukeboxBlue","JukeboxOrange","CouchBlue",
        "CouchPink","CouchWhite","Boombox","FireworkMissile","Snowflake",
        "SpookyCandle5","MineralDiamond","TractorGreen","TractorOrange","TractorRed"
    }
}

-- フェザー設定
local FeatherConfig = {
    Enabled=false, spacing=3, heightOffset=2, backwardOffset=3,
    maxSparklers=20, tiltAngle=45, waveSpeed=2, baseAmplitude=1,
}
local FeatherToys={}; local FeatherRowPoints={}; local FeatherAssignedToys={}
local FeatherLoopConn=nil; local FeatherTime=0

-- 魔法陣設定
local MagicCircleConfig = {
    Enabled=false, Height=5.0, Diameter=5.0, ObjectCount=10,
    RotationSpeed=20.0, SymbolType="Ring", GlowEffect=true, GlowIntensity=1.0,
}
local MagicCircleList={}; local MagicCircleLoopConn=nil; local MagicCircleTAccum=0

-- ♡ハート// 設定
local HeartConfig2 = {
    Enabled=false, Height=5.0, Size=5.0, ObjectCount=12,
    RotationSpeed=1.0, PulseSpeed=2.0, PulseAmplitude=0.5, FollowPlayer=true,
}
local HeartToys2={}; local HeartPoints2={}; local HeartAssignedToys2={}
local HeartLoopConn2=nil; local HeartTime2=0

-- おっきぃ♡ 設定
local BigHeartConfig = {
    Enabled=false, Height=8.0, Size=10.0, ObjectCount=20,
    RotationSpeed=0.5, RotationSpeedMax=10.0, PulseSpeed=1.0,
    PulseSpeedMax=10.0, PulseAmplitude=1.0, FollowPlayer=true,
    HeartScale=2.0, VerticalStretch=1.2,
}
local BigHeartToys={}; local BigHeartPoints={}; local BigHeartAssignedToys={}
local BigHeartLoopConn=nil; local BigHeartTime=0

-- ダビデの星設定
local StarOfDavidConfig = {
    Enabled=false, Height=5.0, Size=5.0, ObjectCount=12,
    RotationSpeed=1.0, PulseSpeed=1.5, FollowPlayer=true, TriangleHeight=0.5,
}
local StarOfDavidToys={}; local StarOfDavidPoints={}; local StarOfDavidAssignedToys={}
local StarOfDavidLoopConn=nil; local StarOfDavidTime=0

-- スター設定
local StarConfig = {
    Enabled=false, Height=5.0, Size=5.0, ObjectCount=10,
    RotationSpeed=1.0, TwinkleSpeed=2.0, FollowPlayer=true,
    StarPoints=5, OuterRadius=5.0, InnerRadius=2.0,
}
local StarToys={}; local StarPoints2={}; local StarAssignedToys={}
local StarLoopConn=nil; local StarTime=0

-- スター2設定
local Star2Config = {
    Enabled=false, Height=10.0, Size=15.0, ObjectCount=24,
    RotationSpeed=5.0, RotationSpeedMax=30.0, PulseSpeed=8.0,
    PulseSpeedMax=20.0, PulseAmplitude=2.0, FollowPlayer=true,
    RayCount=12, RayLength=3.0, RayLengthMax=10.0, JitterSpeed=5.0,
    JitterAmount=1.0, SizeMax=30.0, MaxDistance=50.0,
}
local Star2Toys={}; local Star2Points={}; local Star2AssignedToys={}
local Star2LoopConn=nil; local Star2Time=0

-- 球体設定
local SphereConfig = {
    Enabled=false, BaseHeight=0, Radius=5.0, ObjectCount=20,
    HorizontalRotationSpeed=2.0, VerticalRotationSpeed=1.0, SpiralSpeed=0.5,
    WaveSpeed=1.0, WaveAmplitude=0.3, FollowPlayer=true,
    Latitudes=3, Longitudes=6, PulseSpeed=1.0, PulseAmplitude=0.5,
}
local SphereToys={}; local SpherePoints={}; local SphereAssignedToys={}
local SphereLoopConn=nil; local SphereTime=0

-- 観覧車設定
local FerrisWheelConfig = {
    Enabled=false, Height=15.0, Radius=10.0, ObjectCount=12,
    RotationSpeed=1.0, RotationSpeedMax=5.0, FollowPlayer=true,
    VerticalCircle=true, TiltAngle=0, PulseEffect=false,
    PulseSpeed=1.0, PulseAmplitude=2.0, SwingEffect=false, SwingAmount=0.5,
    FixedDirection=true, FixedYaw=0, FixedPitch=0, FixedRoll=0,
}
local FerrisWheelToys={}; local FerrisWheelPoints={}; local FerrisWheelAssignedToys={}
local FerrisWheelLoopConn=nil; local FerrisWheelTime=0

-- アニメーションN1 (カオス・サークル)
local AnimN1Config = {
    Enabled=false, Height=10.0, Radius=15.0, ObjectCount=50,
    RotationSpeed=20.0, PulseSpeed=5.0, PulseAmount=10.0, FollowPlayer=true,
}
local AnimN1Toys={}; local AnimN1Points={}; local AnimN1AssignedToys={}
local AnimN1LoopConn=nil; local AnimN1Time=0

-- アニメーションN2 (トルネード・スパイラル)
local AnimN2Config = {
    Enabled=false, BaseHeight=5.0, TopHeight=30.0, Radius=8.0, ObjectCount=60,
    RotationSpeed=15.0, RiseSpeed=2.0, ChaosFactor=3.0, FollowPlayer=true,
}
local AnimN2Toys={}; local AnimN2Points={}; local AnimN2AssignedToys={}
local AnimN2LoopConn=nil; local AnimN2Time=0

-- アニメーションN3 (ハイパー・エクスプロージョン)
local AnimN3Config = {
    Enabled=false, Height=8.0, ExplosionRadius=25.0, ObjectCount=80,
    CycleSpeed=2.0, ExplosionSpeed=10.0, Randomness=5.0, FollowPlayer=true,
}
local AnimN3Toys={}; local AnimN3Points={}; local AnimN3AssignedToys={}
local AnimN3LoopConn=nil; local AnimN3Time=0

-- TPWalk / シンプルESP 設定
local UtilityConfig = {
    TPWalk=false, TPWalkSpeed=50, TPWalkSpeedMax=500,
    SimpleESP=false, FOV2=70, OriginalFOV2=70,
}
local TPWalkConnection2=nil; local OriginalWalkSpeed2=16
local SimpleESPConnection=nil; local SimpleESPLabels={}

-- ====================================================================
-- さくらhub 共通ユーティリティ関数
-- ====================================================================
local function findObjects()
    local toys={}
    for _, item in ipairs(workspace:GetDescendants()) do
        if item:IsA("Model") and item.Name==ObjectIDConfig.CurrentObjectID then
            local ok=false
            for _,e in ipairs(toys) do if e==item then ok=true; break end end
            if not ok then table.insert(toys, item) end
        end
    end
    return toys
end

local function getPrimaryPart(model)
    if model.PrimaryPart then return model.PrimaryPart end
    local names={"Handle","Main","Part","Base","Sparkler","Firework","Blade","Candle",
        "Keyboard","Box","Decoy","Missile","Ladder","Blob","Ball","Crystal",
        "Island","Ufo","Jukebox","Couch","Boombox","Snowflake","Diamond","Tractor"}
    for _, n in ipairs(names) do
        local p=model:FindFirstChild(n)
        if p and p:IsA("BasePart") then return p end
    end
    for _, c in ipairs(model:GetChildren()) do
        if c:IsA("BasePart") then return c end
    end
    return nil
end

local function attachPhysics(part, pValue, dValue)
    if not part then return nil, nil end
    local eBG=part:FindFirstChildOfClass("BodyGyro")
    local eBP=part:FindFirstChildOfClass("BodyPosition")
    if eBG and eBP then return eBG, eBP end
    if eBG then eBG:Destroy() end
    if eBP then eBP:Destroy() end
    local BP=Instance.new("BodyPosition")
    local BG=Instance.new("BodyGyro")
    BP.P=pValue or 15000; BP.D=dValue or 200
    BP.MaxForce=Vector3.new(1,1,1)*1e10; BP.Parent=part
    BG.P=pValue or 15000; BG.D=dValue or 200
    BG.MaxTorque=Vector3.new(1,1,1)*1e10; BG.Parent=part
    return BG, BP
end

local function disableCollisionAll(toy)
    for _, c in ipairs(toy:GetChildren()) do
        if c:IsA("BasePart") then c.CanCollide=false; c.CanTouch=false; c.Anchored=false end
    end
end

-- forward-declare toggles (mutal exclusion)
local toggleFeather, toggleMagicCircle, toggleHeart2, toggleBigHeart
local toggleStarOfDavid, toggleStar, toggleStar2, toggleSphere
local toggleFerrisWheel, toggleAnimN1, toggleAnimN2, toggleAnimN3

local function stopAllAnimations()
    if FeatherConfig.Enabled then toggleFeather(false) end
    if MagicCircleConfig.Enabled then toggleMagicCircle(false) end
    if HeartConfig2.Enabled then toggleHeart2(false) end
    if BigHeartConfig.Enabled then toggleBigHeart(false) end
    if StarOfDavidConfig.Enabled then toggleStarOfDavid(false) end
    if StarConfig.Enabled then toggleStar(false) end
    if Star2Config.Enabled then toggleStar2(false) end
    if SphereConfig.Enabled then toggleSphere(false) end
    if FerrisWheelConfig.Enabled then toggleFerrisWheel(false) end
    if AnimN1Config.Enabled then toggleAnimN1(false) end
    if AnimN2Config.Enabled then toggleAnimN2(false) end
    if AnimN3Config.Enabled then toggleAnimN3(false) end
end

local function changeObjectID(id)
    if id==ObjectIDConfig.CurrentObjectID then return end
    ObjectIDConfig.CurrentObjectID=id
    local wasActive={}
    if FeatherConfig.Enabled then table.insert(wasActive,toggleFeather) end
    if MagicCircleConfig.Enabled then table.insert(wasActive,toggleMagicCircle) end
    if HeartConfig2.Enabled then table.insert(wasActive,toggleHeart2) end
    if BigHeartConfig.Enabled then table.insert(wasActive,toggleBigHeart) end
    if StarOfDavidConfig.Enabled then table.insert(wasActive,toggleStarOfDavid) end
    if StarConfig.Enabled then table.insert(wasActive,toggleStar) end
    if Star2Config.Enabled then table.insert(wasActive,toggleStar2) end
    if SphereConfig.Enabled then table.insert(wasActive,toggleSphere) end
    if FerrisWheelConfig.Enabled then table.insert(wasActive,toggleFerrisWheel) end
    if AnimN1Config.Enabled then table.insert(wasActive,toggleAnimN1) end
    if AnimN2Config.Enabled then table.insert(wasActive,toggleAnimN2) end
    if AnimN3Config.Enabled then table.insert(wasActive,toggleAnimN3) end
    stopAllAnimations()
    task.wait(0.5)
    for _,f in ipairs(wasActive) do f(true) end
    OrionLib:MakeNotification({Name="オブジェクトID変更",Content=id.." に切り替えました",Time=3})
end

-- ====================================================================
-- フェザー関数
-- ====================================================================
local function createFeatherRowPoints(count)
    local points={}
    if count==0 then return points end
    local half=math.floor(count/2)
    local odd=count%2==1
    for i=1,count do
        local x=odd and (i-math.ceil(count/2))*FeatherConfig.spacing or (i-half-0.5)*FeatherConfig.spacing
        local p=Instance.new("Part")
        p.CanCollide=false;p.Anchored=true;p.Transparency=1
        p.Size=Vector3.new(4,1,4);p.Parent=workspace
        points[i]={offsetX=x,part=p,assignedToy=nil}
    end
    return points
end

local function assignFeatherToysToPoints()
    FeatherAssignedToys={}
    local dg={}
    for i,pt in ipairs(FeatherRowPoints) do
        local abs=math.abs(pt.offsetX)
        if not dg[abs] then dg[abs]={} end
        table.insert(dg[abs],i)
    end
    local sd={}; for d in pairs(dg) do table.insert(sd,d) end; table.sort(sd)
    for rank,d in ipairs(sd) do
        for _,pi in ipairs(dg[d]) do FeatherRowPoints[pi].distanceRank=rank end
    end
    for i=1,math.min(#FeatherToys,#FeatherRowPoints) do
        local toy=FeatherToys[i]
        if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
            local pp=getPrimaryPart(toy)
            if pp then
                disableCollisionAll(toy)
                local BG,BP=attachPhysics(pp)
                local t={BG=BG,BP=BP,Pallet=pp,Model=toy,RowIndex=i,
                    offsetX=FeatherRowPoints[i].offsetX,distanceRank=FeatherRowPoints[i].distanceRank}
                FeatherRowPoints[i].assignedToy=t
                table.insert(FeatherAssignedToys,t)
            end
        end
    end
    return FeatherAssignedToys
end

local function startFeatherLoop()
    if FeatherLoopConn then FeatherLoopConn:Disconnect();FeatherLoopConn=nil end
    FeatherTime=0
    FeatherLoopConn=RunService.RenderStepped:Connect(function(dt)
        if not FeatherConfig.Enabled or not LocalPlayer.Character then return end
        local hrp=LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not hrp or not torso then return end
        FeatherTime=FeatherTime+dt*FeatherConfig.waveSpeed
        local cf=hrp.CFrame
        local rv=cf.RightVector; local lv=cf.LookVector; local bv=-lv
        local base=torso.Position+Vector3.new(0,FeatherConfig.heightOffset,0)+(bv*FeatherConfig.backwardOffset)
        for _,pt in ipairs(FeatherRowPoints) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local tp=base+(rv*toy.offsetX)
                local amp=FeatherConfig.baseAmplitude*toy.distanceRank
                local wm=math.sin(FeatherTime)*amp
                local fp=tp+Vector3.new(0,wm,0)
                if pt.part then pt.part.Position=fp end
                toy.BP.Position=fp
                local yaw=math.atan2(-lv.X,-lv.Z)
                local bCF=CFrame.new(fp)*CFrame.Angles(0,yaw,0)
                local tCF=bCF*CFrame.Angles(math.rad(-FeatherConfig.tiltAngle),0,0)
                toy.BG.CFrame=toy.BG.CFrame:Lerp(tCF,0.3)
            end
        end
    end)
end

local function stopFeatherLoop()
    if FeatherLoopConn then FeatherLoopConn:Disconnect();FeatherLoopConn=nil end
    for _,pt in ipairs(FeatherRowPoints) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    FeatherRowPoints={}; FeatherAssignedToys={}
end

toggleFeather = function(state)
    FeatherConfig.Enabled=state
    if state then
        stopAllAnimations(); FeatherConfig.Enabled=true
        FeatherToys=findObjects()
        FeatherRowPoints=createFeatherRowPoints(math.min(#FeatherToys,FeatherConfig.maxSparklers))
        FeatherAssignedToys=assignFeatherToysToPoints()
        startFeatherLoop()
        OrionLib:MakeNotification({Name="フェザー開始",Content="数: "..#FeatherAssignedToys,Time=3})
    else
        stopFeatherLoop()
        OrionLib:MakeNotification({Name="フェザー停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- 魔法陣関数
-- ====================================================================
local function attachMagicPhysics(rec)
    local model=rec.model; local part=rec.part
    if not model or not part or not part.Parent then return end
    for _,p in ipairs(model:GetDescendants()) do
        if p:IsA("BasePart") then
            p.CanCollide=false; p.CanTouch=false
            if MagicCircleConfig.GlowEffect then p.Material=Enum.Material.Neon end
        end
    end
    if not part:FindFirstChild("MCBodyVelocity") then
        local bv=Instance.new("BodyVelocity"); bv.Name="MCBodyVelocity"
        bv.MaxForce=Vector3.new(1e8,1e8,1e8); bv.Velocity=Vector3.new(); bv.P=1e6; bv.Parent=part
    end
    if not part:FindFirstChild("MCBodyGyro") then
        local bg=Instance.new("BodyGyro"); bg.Name="MCBodyGyro"
        bg.MaxTorque=Vector3.new(1e8,1e8,1e8); bg.CFrame=part.CFrame; bg.P=1e6; bg.Parent=part
    end
end

local function detachMagicPhysics(rec)
    local part=rec.part; if not part then return end
    local bv=part:FindFirstChild("MCBodyVelocity"); if bv then bv:Destroy() end
    local bg=part:FindFirstChild("MCBodyGyro"); if bg then bg:Destroy() end
    if rec.model then
        for _,p in ipairs(rec.model:GetDescendants()) do
            if p:IsA("BasePart") then p.CanCollide=true; p.CanTouch=true; p.Material=Enum.Material.Plastic end
        end
    end
end

local function rescanMagicCircle()
    for _,r in ipairs(MagicCircleList) do detachMagicPhysics(r) end
    MagicCircleList={}
    local n=0
    for _,d in ipairs(workspace:GetDescendants()) do
        if n>=MagicCircleConfig.ObjectCount then break end
        if d:IsA("Model") and d.Name==ObjectIDConfig.CurrentObjectID then
            local part=getPrimaryPart(d)
            if part and not part.Anchored then
                table.insert(MagicCircleList,{model=d,part=part}); n=n+1
            end
        end
    end
    for _,r in ipairs(MagicCircleList) do attachMagicPhysics(r) end
end

local function startMagicCircleLoop()
    if MagicCircleLoopConn then MagicCircleLoopConn:Disconnect();MagicCircleLoopConn=nil end
    MagicCircleTAccum=0
    MagicCircleLoopConn=RunService.Heartbeat:Connect(function(dt)
        if not MagicCircleConfig.Enabled then return end
        local root=HRP(); if not root or #MagicCircleList==0 then return end
        MagicCircleTAccum=MagicCircleTAccum+dt*(MagicCircleConfig.RotationSpeed/10)
        local radius=MagicCircleConfig.Diameter/2
        local ai=360/#MagicCircleList
        local rv=root.AssemblyLinearVelocity or Vector3.new()
        for i,rec in ipairs(MagicCircleList) do
            local part=rec.part; if not part or not part.Parent then continue end
            local angle=math.rad(i*ai+MagicCircleTAccum*50)
            local ho=0
            if MagicCircleConfig.SymbolType=="Hexagram" then
                ho=0.5*math.sin(angle*3)
            elseif MagicCircleConfig.SymbolType=="Circle" then
                ho=math.sin(angle*4)*0.3
            end
            local tp=root.Position+Vector3.new(radius*math.cos(angle),MagicCircleConfig.Height+ho,radius*math.sin(angle))
            local dir=tp-part.Position; local dist=dir.Magnitude
            local bv=part:FindFirstChild("MCBodyVelocity")
            if bv then
                bv.Velocity=(dist>0.1) and dir.Unit*math.min(3000,dist*50)+rv or rv
            end
            local bg=part:FindFirstChild("MCBodyGyro")
            if bg then bg.CFrame=CFrame.lookAt(tp,root.Position)*CFrame.Angles(0,math.pi,0) end
        end
    end)
end

local function stopMagicCircleLoop()
    if MagicCircleLoopConn then MagicCircleLoopConn:Disconnect();MagicCircleLoopConn=nil end
    for _,rec in ipairs(MagicCircleList) do detachMagicPhysics(rec) end
    MagicCircleList={}
end

toggleMagicCircle = function(state)
    MagicCircleConfig.Enabled=state
    if state then
        stopAllAnimations(); MagicCircleConfig.Enabled=true
        rescanMagicCircle(); startMagicCircleLoop()
        OrionLib:MakeNotification({Name="魔法陣開始",Content="高さ:"..MagicCircleConfig.Height,Time=3})
    else
        stopMagicCircleLoop()
        OrionLib:MakeNotification({Name="魔法陣停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- ♡ハート// 関数
-- ====================================================================
local function getHeartPos(t, size, pulse)
    local sc=size/20
    local x=16*(math.sin(t)^3)*sc
    local y=(13*math.cos(t)-5*math.cos(2*t)-2*math.cos(3*t)-math.cos(4*t))*sc
    if pulse~=0 then local f=1+pulse*0.1; x=x*f; y=y*f end
    return x, y
end

local function startHeartLoop2()
    if HeartLoopConn2 then HeartLoopConn2:Disconnect();HeartLoopConn2=nil end
    HeartTime2=0
    HeartLoopConn2=RunService.RenderStepped:Connect(function(dt)
        if not HeartConfig2.Enabled or not LocalPlayer.Character then return end
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not torso then return end
        HeartTime2=HeartTime2+dt
        local base=torso.Position
        local pulse=(HeartConfig2.PulseSpeed>0) and math.sin(HeartTime2*HeartConfig2.PulseSpeed)*HeartConfig2.PulseAmplitude or 0
        for _,pt in ipairs(HeartPoints2) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local ca=toy.baseAngle+(HeartTime2*HeartConfig2.RotationSpeed)
                local x,y=getHeartPos(ca,HeartConfig2.Size,pulse)
                local ho=HeartConfig2.Height+(math.sin(ca*2)*0.5)
                local tp=base+Vector3.new(x,ho,y)
                if pt.part then pt.part.Position=tp end
                toy.BP.Position=tp
                toy.BG.CFrame=CFrame.new(tp)*CFrame.Angles(-math.rad(90),0,0)
            end
        end
    end)
end

local function stopHeartLoop2()
    if HeartLoopConn2 then HeartLoopConn2:Disconnect();HeartLoopConn2=nil end
    for _,pt in ipairs(HeartPoints2) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    HeartPoints2={}; HeartAssignedToys2={}
end

toggleHeart2 = function(state)
    HeartConfig2.Enabled=state
    if state then
        stopAllAnimations(); HeartConfig2.Enabled=true
        HeartToys2=findObjects()
        HeartPoints2={}
        for i=1,math.min(#HeartToys2,HeartConfig2.ObjectCount) do
            local p=Instance.new("Part"); p.CanCollide=false; p.Anchored=true
            p.Transparency=1; p.Size=Vector3.new(4,1,4); p.Parent=workspace
            HeartPoints2[i]={angle=(i-1)*(2*math.pi/HeartConfig2.ObjectCount),part=p,assignedToy=nil}
        end
        HeartAssignedToys2={}
        for i=1,math.min(#HeartToys2,#HeartPoints2) do
            local toy=HeartToys2[i]
            if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
                local pp=getPrimaryPart(toy)
                if pp then
                    disableCollisionAll(toy)
                    local BG,BP=attachPhysics(pp)
                    local t={BG=BG,BP=BP,Pallet=pp,Model=toy,baseAngle=HeartPoints2[i].angle}
                    HeartPoints2[i].assignedToy=t; table.insert(HeartAssignedToys2,t)
                end
            end
        end
        startHeartLoop2()
        OrionLib:MakeNotification({Name="♡ハート開始",Content="数:"..HeartConfig2.ObjectCount,Time=3})
    else
        stopHeartLoop2()
        OrionLib:MakeNotification({Name="♡ハート停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- おっきぃ♡ 関数
-- ====================================================================
local function startBigHeartLoop()
    if BigHeartLoopConn then BigHeartLoopConn:Disconnect();BigHeartLoopConn=nil end
    BigHeartTime=0
    BigHeartLoopConn=RunService.RenderStepped:Connect(function(dt)
        if not BigHeartConfig.Enabled or not LocalPlayer.Character then return end
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not torso then return end
        BigHeartTime=BigHeartTime+dt
        local base=torso.Position
        local pulse=(BigHeartConfig.PulseSpeed>0) and math.sin(BigHeartTime*BigHeartConfig.PulseSpeed)*BigHeartConfig.PulseAmplitude or 0
        for _,pt in ipairs(BigHeartPoints) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local ca=toy.baseAngle+(BigHeartTime*BigHeartConfig.RotationSpeed)
                local sc=BigHeartConfig.Size/20*BigHeartConfig.HeartScale
                local x=16*(math.sin(ca)^3)*sc
                local y=(13*math.cos(ca)-5*math.cos(2*ca)-2*math.cos(3*ca)-math.cos(ca*4))*sc*BigHeartConfig.VerticalStretch
                if pulse~=0 then local f=1+pulse*0.1; x=x*f; y=y*f end
                local ho=BigHeartConfig.Height+(math.sin(ca*2)*1.0)
                local tp=base+Vector3.new(x,ho,y)
                if pt.part then pt.part.Position=tp end
                toy.BP.Position=tp
                toy.BG.CFrame=CFrame.new(tp)*CFrame.Angles(-math.rad(90),0,0)
            end
        end
    end)
end

local function stopBigHeartLoop()
    if BigHeartLoopConn then BigHeartLoopConn:Disconnect();BigHeartLoopConn=nil end
    for _,pt in ipairs(BigHeartPoints) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    BigHeartPoints={}; BigHeartAssignedToys={}
end

toggleBigHeart = function(state)
    BigHeartConfig.Enabled=state
    if state then
        stopAllAnimations(); BigHeartConfig.Enabled=true
        BigHeartToys=findObjects()
        BigHeartPoints={}
        for i=1,math.min(#BigHeartToys,BigHeartConfig.ObjectCount) do
            local p=Instance.new("Part"); p.CanCollide=false; p.Anchored=true
            p.Transparency=1; p.Size=Vector3.new(4,1,4); p.Parent=workspace
            BigHeartPoints[i]={angle=(i-1)*(2*math.pi/BigHeartConfig.ObjectCount),part=p,assignedToy=nil}
        end
        BigHeartAssignedToys={}
        for i=1,math.min(#BigHeartToys,#BigHeartPoints) do
            local toy=BigHeartToys[i]
            if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
                local pp=getPrimaryPart(toy)
                if pp then
                    disableCollisionAll(toy)
                    local BG,BP=attachPhysics(pp)
                    local t={BG=BG,BP=BP,Pallet=pp,Model=toy,baseAngle=BigHeartPoints[i].angle}
                    BigHeartPoints[i].assignedToy=t; table.insert(BigHeartAssignedToys,t)
                end
            end
        end
        startBigHeartLoop()
        OrionLib:MakeNotification({Name="おっきぃ♡開始",Content=BigHeartConfig.Size.."×"..BigHeartConfig.HeartScale,Time=3})
    else
        stopBigHeartLoop()
        OrionLib:MakeNotification({Name="おっきぃ♡停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- ダビデの星 関数
-- ====================================================================
local function startStarOfDavidLoop()
    if StarOfDavidLoopConn then StarOfDavidLoopConn:Disconnect();StarOfDavidLoopConn=nil end
    StarOfDavidTime=0
    StarOfDavidLoopConn=RunService.RenderStepped:Connect(function(dt)
        if not StarOfDavidConfig.Enabled or not LocalPlayer.Character then return end
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not torso then return end
        StarOfDavidTime=StarOfDavidTime+dt
        local base=torso.Position
        for i,pt in ipairs(StarOfDavidPoints) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local ca=toy.baseAngle+(StarOfDavidTime*StarOfDavidConfig.RotationSpeed)
                local sc=StarOfDavidConfig.Size/10
                local bx=math.cos(ca)*sc; local bz=math.sin(ca)*sc
                local ho=(i%2==0) and StarOfDavidConfig.TriangleHeight or -StarOfDavidConfig.TriangleHeight
                local pulse=math.sin(StarOfDavidTime*StarOfDavidConfig.PulseSpeed)*0.1
                local fh=StarOfDavidConfig.Height+ho+pulse
                local tp=base+Vector3.new(bx,fh,bz)
                if pt.part then pt.part.Position=tp end
                toy.BP.Position=tp
                local dir=(tp-base).Unit
                if dir.Magnitude>0 then toy.BG.CFrame=CFrame.lookAt(tp,tp+dir) end
            end
        end
    end)
end

local function stopStarOfDavidLoop()
    if StarOfDavidLoopConn then StarOfDavidLoopConn:Disconnect();StarOfDavidLoopConn=nil end
    for _,pt in ipairs(StarOfDavidPoints) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    StarOfDavidPoints={}; StarOfDavidAssignedToys={}
end

toggleStarOfDavid = function(state)
    StarOfDavidConfig.Enabled=state
    if state then
        stopAllAnimations(); StarOfDavidConfig.Enabled=true
        StarOfDavidToys=findObjects()
        StarOfDavidPoints={}
        for i=1,math.min(#StarOfDavidToys,StarOfDavidConfig.ObjectCount) do
            local angle=(i-1)*(2*math.pi/6)
            local p=Instance.new("Part"); p.CanCollide=false; p.Anchored=true
            p.Transparency=1; p.Size=Vector3.new(4,1,4); p.Parent=workspace
            StarOfDavidPoints[i]={angle=angle,part=p,assignedToy=nil}
        end
        StarOfDavidAssignedToys={}
        for i=1,math.min(#StarOfDavidToys,#StarOfDavidPoints) do
            local toy=StarOfDavidToys[i]
            if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
                local pp=getPrimaryPart(toy)
                if pp then
                    disableCollisionAll(toy)
                    local BG,BP=attachPhysics(pp)
                    local t={BG=BG,BP=BP,Pallet=pp,Model=toy,baseAngle=StarOfDavidPoints[i].angle}
                    StarOfDavidPoints[i].assignedToy=t; table.insert(StarOfDavidAssignedToys,t)
                end
            end
        end
        startStarOfDavidLoop()
        OrionLib:MakeNotification({Name="ダビデの星開始",Content="サイズ:"..StarOfDavidConfig.Size,Time=3})
    else
        stopStarOfDavidLoop()
        OrionLib:MakeNotification({Name="ダビデの星停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- スター★ 関数
-- ====================================================================
local function startStarLoop()
    if StarLoopConn then StarLoopConn:Disconnect();StarLoopConn=nil end
    StarTime=0
    StarLoopConn=RunService.RenderStepped:Connect(function(dt)
        if not StarConfig.Enabled or not LocalPlayer.Character then return end
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not torso then return end
        StarTime=StarTime+dt
        local base=torso.Position
        for _,pt in ipairs(StarPoints2) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local si=toy.starIndex; local isOuter=toy.isOuter
                local ape=2*math.pi/5
                local pa=si*(ape/2)
                local radius=(isOuter and StarConfig.OuterRadius) or StarConfig.InnerRadius
                local ra=pa+(StarTime*StarConfig.RotationSpeed)
                local twinkle=math.sin(StarTime*StarConfig.TwinkleSpeed+si)*0.2
                local fr=radius*(1+twinkle)
                local x=math.cos(ra)*fr; local z=math.sin(ra)*fr
                local hv=math.sin(pa*3)*0.5
                local tp=base+Vector3.new(x,StarConfig.Height+hv,z)
                if pt.part then pt.part.Position=tp end
                toy.BP.Position=tp
                local dir=(tp-base).Unit
                if dir.Magnitude>0 then toy.BG.CFrame=CFrame.lookAt(tp,tp+dir) end
            end
        end
    end)
end

local function stopStarLoop()
    if StarLoopConn then StarLoopConn:Disconnect();StarLoopConn=nil end
    for _,pt in ipairs(StarPoints2) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    StarPoints2={}; StarAssignedToys={}
end

toggleStar = function(state)
    StarConfig.Enabled=state
    if state then
        stopAllAnimations(); StarConfig.Enabled=true
        StarToys=findObjects(); StarPoints2={}; StarAssignedToys={}
        for i=1,math.min(#StarToys,StarConfig.ObjectCount) do
            local si=(i-1)%10; local isO=si%2==0
            local p=Instance.new("Part"); p.CanCollide=false; p.Anchored=true
            p.Transparency=1; p.Size=Vector3.new(4,1,4); p.Parent=workspace
            StarPoints2[i]={starIndex=si,isOuter=isO,part=p,assignedToy=nil}
        end
        for i=1,math.min(#StarToys,#StarPoints2) do
            local toy=StarToys[i]
            if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
                local pp=getPrimaryPart(toy)
                if pp then
                    disableCollisionAll(toy)
                    local BG,BP=attachPhysics(pp)
                    local t={BG=BG,BP=BP,Pallet=pp,Model=toy,starIndex=StarPoints2[i].starIndex,isOuter=StarPoints2[i].isOuter}
                    StarPoints2[i].assignedToy=t; table.insert(StarAssignedToys,t)
                end
            end
        end
        startStarLoop()
        OrionLib:MakeNotification({Name="スター★開始",Content="外径:"..StarConfig.OuterRadius,Time=3})
    else
        stopStarLoop()
        OrionLib:MakeNotification({Name="スター★停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- スター2✫ 関数
-- ====================================================================
local function startStar2Loop()
    if Star2LoopConn then Star2LoopConn:Disconnect();Star2LoopConn=nil end
    Star2Time=0
    Star2LoopConn=RunService.RenderStepped:Connect(function(dt)
        if not Star2Config.Enabled or not LocalPlayer.Character then return end
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not torso then return end
        Star2Time=Star2Time+dt
        local base=torso.Position
        local pulse=(Star2Config.PulseSpeed>0) and math.sin(Star2Time*Star2Config.PulseSpeed)*Star2Config.PulseAmplitude or 0
        for _,pt in ipairs(Star2Points) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local ca=toy.baseAngle+(Star2Time*Star2Config.RotationSpeed)
                local sc=Star2Config.Size/10
                local ape=2*math.pi/Star2Config.RayCount
                local ra=toy.rayIndex*ape
                local ad=math.abs(ca-ra); if ad>math.pi then ad=2*math.pi-ad end
                local rf=0
                if ad<(ape/4) then
                    rf=(1-(ad/(ape/4)))*Star2Config.RayLength
                    rf=rf*(1+math.sin(Star2Time*Star2Config.JitterSpeed+toy.rayIndex)*Star2Config.JitterAmount*0.1)
                end
                local pf=1+pulse*0.1
                local fr=math.min((sc+rf)*pf,Star2Config.MaxDistance)
                local x=math.cos(ca)*fr; local z=math.sin(ca)*fr
                local hv=math.sin(Star2Time*3+toy.rayIndex)*0.5
                local tp=base+Vector3.new(x,Star2Config.Height+hv,z)
                if pt.part then pt.part.Position=tp end
                toy.BP.Position=tp
                local dir=(tp-base).Unit
                if dir.Magnitude>0 then toy.BG.CFrame=toy.BG.CFrame:Lerp(CFrame.lookAt(tp,tp+dir),0.5) end
            end
        end
    end)
end

local function stopStar2Loop()
    if Star2LoopConn then Star2LoopConn:Disconnect();Star2LoopConn=nil end
    for _,pt in ipairs(Star2Points) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    Star2Points={}; Star2AssignedToys={}
end

toggleStar2 = function(state)
    Star2Config.Enabled=state
    if state then
        stopAllAnimations(); Star2Config.Enabled=true
        Star2Toys=findObjects(); Star2Points={}; Star2AssignedToys={}
        for i=1,math.min(#Star2Toys,Star2Config.ObjectCount) do
            local t=(i-1)*(2*math.pi/Star2Config.ObjectCount)
            local p=Instance.new("Part"); p.CanCollide=false; p.Anchored=true
            p.Transparency=1; p.Size=Vector3.new(4,1,4); p.Parent=workspace
            Star2Points[i]={angle=t,part=p,assignedToy=nil,rayIndex=(i-1)%Star2Config.RayCount}
        end
        for i=1,math.min(#Star2Toys,#Star2Points) do
            local toy=Star2Toys[i]
            if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
                local pp=getPrimaryPart(toy)
                if pp then
                    disableCollisionAll(toy)
                    local BG,BP=attachPhysics(pp,20000,300)
                    local t={BG=BG,BP=BP,Pallet=pp,Model=toy,baseAngle=Star2Points[i].angle,rayIndex=Star2Points[i].rayIndex}
                    Star2Points[i].assignedToy=t; table.insert(Star2AssignedToys,t)
                end
            end
        end
        startStar2Loop()
        OrionLib:MakeNotification({Name="スター2✫開始",Content="サイズ:"..Star2Config.Size,Time=3})
    else
        stopStar2Loop()
        OrionLib:MakeNotification({Name="スター2✫停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- 球体◯ 関数
-- ====================================================================
local function startSphereLoop()
    if SphereLoopConn then SphereLoopConn:Disconnect();SphereLoopConn=nil end
    SphereTime=0
    SphereLoopConn=RunService.RenderStepped:Connect(function(dt)
        if not SphereConfig.Enabled or not LocalPlayer.Character then return end
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not torso then return end
        SphereTime=SphereTime+dt
        local base=torso.Position
        for _,pt in ipairs(SpherePoints) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local lats=SphereConfig.Latitudes; local lons=SphereConfig.Longitudes
                local la=((toy.latIndex-1)/(lats-1)-0.5)*math.pi
                local lo=((toy.lonIndex-1)/lons)*2*math.pi
                local rlo=lo+(SphereTime*SphereConfig.HorizontalRotationSpeed)
                local rla=la+(SphereTime*SphereConfig.VerticalRotationSpeed*0.5)
                local pulse=(SphereConfig.PulseSpeed>0) and 1+math.sin(SphereTime*SphereConfig.PulseSpeed)*SphereConfig.PulseAmplitude or 1
                local wave=(SphereConfig.WaveSpeed>0) and 1+math.sin(SphereTime*SphereConfig.WaveSpeed+toy.lonIndex*0.5)*SphereConfig.WaveAmplitude or 1
                local fr=SphereConfig.Radius*pulse*wave
                local x=fr*math.cos(rla)*math.cos(rlo)
                local y=fr*math.sin(rla)
                local z=fr*math.cos(rla)*math.sin(rlo)
                local tp=base+Vector3.new(x,SphereConfig.BaseHeight+y,z)
                if pt.part then pt.part.Position=tp end
                toy.BP.Position=tp
                local dir=(tp-base).Unit
                if dir.Magnitude>0 then toy.BG.CFrame=toy.BG.CFrame:Lerp(CFrame.lookAt(tp,tp+dir),0.4) end
            end
        end
    end)
end

local function stopSphereLoop()
    if SphereLoopConn then SphereLoopConn:Disconnect();SphereLoopConn=nil end
    for _,pt in ipairs(SpherePoints) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    SpherePoints={}; SphereAssignedToys={}
end

toggleSphere = function(state)
    SphereConfig.Enabled=state
    if state then
        stopAllAnimations(); SphereConfig.Enabled=true
        SphereToys=findObjects(); SpherePoints={}; SphereAssignedToys={}
        for i=1,math.min(#SphereToys,SphereConfig.ObjectCount) do
            local li=math.floor((i-1)/SphereConfig.Longitudes)+1
            local ni=((i-1)%SphereConfig.Longitudes)+1
            local p=Instance.new("Part"); p.CanCollide=false; p.Anchored=true
            p.Transparency=1; p.Size=Vector3.new(4,1,4); p.Parent=workspace
            SpherePoints[i]={latIndex=li,lonIndex=ni,part=p,assignedToy=nil}
        end
        for i=1,math.min(#SphereToys,#SpherePoints) do
            local toy=SphereToys[i]
            if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
                local pp=getPrimaryPart(toy)
                if pp then
                    disableCollisionAll(toy)
                    local BG,BP=attachPhysics(pp,20000,300)
                    local t={BG=BG,BP=BP,Pallet=pp,Model=toy,latIndex=SpherePoints[i].latIndex,lonIndex=SpherePoints[i].lonIndex}
                    SpherePoints[i].assignedToy=t; table.insert(SphereAssignedToys,t)
                end
            end
        end
        startSphereLoop()
        OrionLib:MakeNotification({Name="球体◯開始",Content="半径:"..SphereConfig.Radius,Time=3})
    else
        stopSphereLoop()
        OrionLib:MakeNotification({Name="球体◯停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- 観覧車 関数
-- ====================================================================
local function startFerrisWheelLoop()
    if FerrisWheelLoopConn then FerrisWheelLoopConn:Disconnect();FerrisWheelLoopConn=nil end
    FerrisWheelTime=0
    local fixedCF=nil
    if FerrisWheelConfig.FixedDirection then
        fixedCF=CFrame.Angles(math.rad(FerrisWheelConfig.FixedPitch),math.rad(FerrisWheelConfig.FixedYaw),math.rad(FerrisWheelConfig.FixedRoll))
    end
    FerrisWheelLoopConn=RunService.RenderStepped:Connect(function(dt)
        if not FerrisWheelConfig.Enabled or not LocalPlayer.Character then return end
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not torso then return end
        FerrisWheelTime=FerrisWheelTime+dt
        local base=torso.Position
        for _,pt in ipairs(FerrisWheelPoints) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local ca=(toy.baseAngle+(FerrisWheelTime*FerrisWheelConfig.RotationSpeed))%(2*math.pi)
                local cr=FerrisWheelConfig.Radius
                if FerrisWheelConfig.PulseEffect then
                    cr=cr*(1+math.sin(FerrisWheelTime*FerrisWheelConfig.PulseSpeed)*(FerrisWheelConfig.PulseAmplitude/cr))
                end
                local sw=(FerrisWheelConfig.SwingEffect and math.sin(FerrisWheelTime*2+toy.baseAngle)*FerrisWheelConfig.SwingAmount) or 0
                local x,y,z
                if FerrisWheelConfig.VerticalCircle then
                    x=math.cos(ca)*cr; y=math.sin(ca)*cr+FerrisWheelConfig.Height+sw; z=0
                else
                    x=math.cos(ca)*cr; y=FerrisWheelConfig.Height+math.sin(ca*2)*(cr*0.2); z=math.sin(ca)*cr
                end
                local tp=base+Vector3.new(x,y,z)
                if pt.part then pt.part.Position=tp end
                toy.BP.Position=tp
                if FerrisWheelConfig.FixedDirection and fixedCF then
                    toy.BG.CFrame=CFrame.new(tp)*fixedCF
                else
                    toy.BG.CFrame=CFrame.lookAt(tp,base)
                end
            end
        end
    end)
end

local function stopFerrisWheelLoop()
    if FerrisWheelLoopConn then FerrisWheelLoopConn:Disconnect();FerrisWheelLoopConn=nil end
    for _,pt in ipairs(FerrisWheelPoints) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    FerrisWheelPoints={}; FerrisWheelAssignedToys={}
end

toggleFerrisWheel = function(state)
    FerrisWheelConfig.Enabled=state
    if state then
        stopAllAnimations(); FerrisWheelConfig.Enabled=true
        FerrisWheelToys=findObjects(); FerrisWheelPoints={}; FerrisWheelAssignedToys={}
        local as=(2*math.pi)/FerrisWheelConfig.ObjectCount
        for i=1,math.min(#FerrisWheelToys,FerrisWheelConfig.ObjectCount) do
            local p=Instance.new("Part"); p.CanCollide=false; p.Anchored=true
            p.Transparency=1; p.Size=Vector3.new(4,1,4); p.Parent=workspace
            FerrisWheelPoints[i]={baseAngle=(i-1)*as,part=p,assignedToy=nil}
        end
        for i=1,math.min(#FerrisWheelToys,#FerrisWheelPoints) do
            local toy=FerrisWheelToys[i]
            if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
                local pp=getPrimaryPart(toy)
                if pp then
                    disableCollisionAll(toy)
                    local BG,BP=attachPhysics(pp,20000,300)
                    local t={BG=BG,BP=BP,Pallet=pp,Model=toy,baseAngle=FerrisWheelPoints[i].baseAngle}
                    FerrisWheelPoints[i].assignedToy=t; table.insert(FerrisWheelAssignedToys,t)
                end
            end
        end
        startFerrisWheelLoop()
        OrionLib:MakeNotification({Name="観覧車開始",Content="半径:"..FerrisWheelConfig.Radius,Time=3})
    else
        stopFerrisWheelLoop()
        OrionLib:MakeNotification({Name="観覧車停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- アニメーションN1 (カオス・サークル)
-- ====================================================================
local function startAnimN1Loop()
    if AnimN1LoopConn then AnimN1LoopConn:Disconnect();AnimN1LoopConn=nil end
    AnimN1Time=0
    AnimN1LoopConn=RunService.RenderStepped:Connect(function(dt)
        if not AnimN1Config.Enabled or not LocalPlayer.Character then return end
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not torso then return end
        AnimN1Time=AnimN1Time+dt
        local base=torso.Position
        for i,pt in ipairs(AnimN1Points) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local ra=toy.baseAngle+(AnimN1Time*AnimN1Config.RotationSpeed)
                local pulse=math.sin(AnimN1Time*AnimN1Config.PulseSpeed)*AnimN1Config.PulseAmount
                local cr=AnimN1Config.Radius+pulse
                local ch=AnimN1Config.Height+math.sin(AnimN1Time*3+i)*5
                local cx=math.sin(AnimN1Time*2+i)*2; local cy=math.cos(AnimN1Time*2.5+i)*2; local cz=math.sin(AnimN1Time*3+i)*2
                local x=math.cos(ra)*cr+cx; local z=math.sin(ra)*cr+cz; local y=ch+cy
                local tp=base+Vector3.new(x,y,z)
                if pt.part then pt.part.Position=tp end
                toy.BP.Position=tp; toy.BG.CFrame=CFrame.new(tp)
            end
        end
    end)
end

local function stopAnimN1Loop()
    if AnimN1LoopConn then AnimN1LoopConn:Disconnect();AnimN1LoopConn=nil end
    for _,pt in ipairs(AnimN1Points) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    AnimN1Points={}; AnimN1AssignedToys={}
end

toggleAnimN1 = function(state)
    AnimN1Config.Enabled=state
    if state then
        stopAllAnimations(); AnimN1Config.Enabled=true
        AnimN1Toys=findObjects(); AnimN1Points={}; AnimN1AssignedToys={}
        for i=1,math.min(#AnimN1Toys,AnimN1Config.ObjectCount) do
            local a=(i-1)*(2*math.pi/AnimN1Config.ObjectCount)
            local p=Instance.new("Part"); p.CanCollide=false; p.Anchored=true
            p.Transparency=1; p.Size=Vector3.new(4,1,4); p.Parent=workspace
            AnimN1Points[i]={baseAngle=a,part=p,assignedToy=nil}
        end
        for i=1,math.min(#AnimN1Toys,#AnimN1Points) do
            local toy=AnimN1Toys[i]
            if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
                local pp=getPrimaryPart(toy)
                if pp then
                    disableCollisionAll(toy); pp.Material=Enum.Material.Neon
                    local BG,BP=attachPhysics(pp,30000,500)
                    local t={BG=BG,BP=BP,Pallet=pp,Model=toy,baseAngle=AnimN1Points[i].baseAngle}
                    AnimN1Points[i].assignedToy=t; table.insert(AnimN1AssignedToys,t)
                end
            end
        end
        startAnimN1Loop()
        OrionLib:MakeNotification({Name="N1開始",Content="数:"..#AnimN1AssignedToys,Time=3})
    else
        stopAnimN1Loop()
        OrionLib:MakeNotification({Name="N1停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- アニメーションN2 (トルネード・スパイラル)
-- ====================================================================
local function startAnimN2Loop()
    if AnimN2LoopConn then AnimN2LoopConn:Disconnect();AnimN2LoopConn=nil end
    AnimN2Time=0
    AnimN2LoopConn=RunService.RenderStepped:Connect(function(dt)
        if not AnimN2Config.Enabled or not LocalPlayer.Character then return end
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not torso then return end
        AnimN2Time=AnimN2Time+dt
        local base=torso.Position
        for i,pt in ipairs(AnimN2Points) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local ra=toy.baseAngle+(AnimN2Time*AnimN2Config.RotationSpeed)
                local rise=(AnimN2Time*AnimN2Config.RiseSpeed)%(AnimN2Config.TopHeight-AnimN2Config.BaseHeight)
                local ch=AnimN2Config.BaseHeight+rise
                local chaos=math.sin(AnimN2Time*AnimN2Config.ChaosFactor+i)*2
                local cr=AnimN2Config.Radius+chaos
                local x=math.cos(ra)*cr+math.sin(AnimN2Time*5+i)*1.5
                local z=math.sin(ra)*cr+math.cos(AnimN2Time*5+i)*1.5
                local tp=base+Vector3.new(x,ch,z)
                if pt.part then pt.part.Position=tp end
                toy.BP.Position=tp; toy.BG.CFrame=CFrame.new(tp)
            end
        end
    end)
end

local function stopAnimN2Loop()
    if AnimN2LoopConn then AnimN2LoopConn:Disconnect();AnimN2LoopConn=nil end
    for _,pt in ipairs(AnimN2Points) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    AnimN2Points={}; AnimN2AssignedToys={}
end

toggleAnimN2 = function(state)
    AnimN2Config.Enabled=state
    if state then
        stopAllAnimations(); AnimN2Config.Enabled=true
        AnimN2Toys=findObjects(); AnimN2Points={}; AnimN2AssignedToys={}
        for i=1,math.min(#AnimN2Toys,AnimN2Config.ObjectCount) do
            local a=(i-1)*(2*math.pi/AnimN2Config.ObjectCount)
            local p=Instance.new("Part"); p.CanCollide=false; p.Anchored=true
            p.Transparency=1; p.Size=Vector3.new(4,1,4); p.Parent=workspace
            AnimN2Points[i]={baseAngle=a,part=p,assignedToy=nil,riseOffset=i/AnimN2Config.ObjectCount}
        end
        for i=1,math.min(#AnimN2Toys,#AnimN2Points) do
            local toy=AnimN2Toys[i]
            if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
                local pp=getPrimaryPart(toy)
                if pp then
                    disableCollisionAll(toy); pp.Material=Enum.Material.Neon
                    local BG,BP=attachPhysics(pp,25000,400)
                    local t={BG=BG,BP=BP,Pallet=pp,Model=toy,baseAngle=AnimN2Points[i].baseAngle,riseOffset=AnimN2Points[i].riseOffset}
                    AnimN2Points[i].assignedToy=t; table.insert(AnimN2AssignedToys,t)
                end
            end
        end
        startAnimN2Loop()
        OrionLib:MakeNotification({Name="N2開始",Content="数:"..#AnimN2AssignedToys,Time=3})
    else
        stopAnimN2Loop()
        OrionLib:MakeNotification({Name="N2停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- アニメーションN3 (ハイパー・エクスプロージョン)
-- ====================================================================
local function startAnimN3Loop()
    if AnimN3LoopConn then AnimN3LoopConn:Disconnect();AnimN3LoopConn=nil end
    AnimN3Time=0
    AnimN3LoopConn=RunService.RenderStepped:Connect(function(dt)
        if not AnimN3Config.Enabled or not LocalPlayer.Character then return end
        local torso=LocalPlayer.Character:FindFirstChild("Torso") or LocalPlayer.Character:FindFirstChild("UpperTorso")
        if not torso then return end
        AnimN3Time=AnimN3Time+dt
        local base=torso.Position
        local cycle=math.sin(AnimN3Time*AnimN3Config.CycleSpeed)
        local ef=(cycle+1)/2
        for i,pt in ipairs(AnimN3Points) do
            if pt.assignedToy and pt.assignedToy.BP and pt.assignedToy.BG then
                local toy=pt.assignedToy
                local dir=Vector3.new(
                    math.sin(AnimN3Time*2+toy.seed),
                    math.cos(AnimN3Time*3+toy.seed),
                    math.sin(AnimN3Time*4+toy.seed)).Unit
                local dist=ef*AnimN3Config.ExplosionRadius+math.sin(AnimN3Time*5+toy.seed)*AnimN3Config.Randomness
                local ho=math.sin(AnimN3Time*3+toy.seed)*5+AnimN3Config.Height
                local lp=dir*dist
                local tp=base+Vector3.new(lp.X,ho,lp.Z)
                if pt.part then pt.part.Position=tp end
                toy.BP.Position=tp; toy.BG.CFrame=CFrame.new(tp)
            end
        end
    end)
end

local function stopAnimN3Loop()
    if AnimN3LoopConn then AnimN3LoopConn:Disconnect();AnimN3LoopConn=nil end
    for _,pt in ipairs(AnimN3Points) do
        if pt.part then pt.part:Destroy() end
        if pt.assignedToy then
            if pt.assignedToy.BG then pt.assignedToy.BG:Destroy() end
            if pt.assignedToy.BP then pt.assignedToy.BP:Destroy() end
        end
    end
    AnimN3Points={}; AnimN3AssignedToys={}
end

toggleAnimN3 = function(state)
    AnimN3Config.Enabled=state
    if state then
        stopAllAnimations(); AnimN3Config.Enabled=true
        AnimN3Toys=findObjects(); AnimN3Points={}; AnimN3AssignedToys={}
        for i=1,math.min(#AnimN3Toys,AnimN3Config.ObjectCount) do
            local p=Instance.new("Part"); p.CanCollide=false; p.Anchored=true
            p.Transparency=1; p.Size=Vector3.new(4,1,4); p.Parent=workspace
            AnimN3Points[i]={part=p,assignedToy=nil,seed=i}
        end
        for i=1,math.min(#AnimN3Toys,#AnimN3Points) do
            local toy=AnimN3Toys[i]
            if toy and toy:IsA("Model") and toy.Name==ObjectIDConfig.CurrentObjectID then
                local pp=getPrimaryPart(toy)
                if pp then
                    disableCollisionAll(toy); pp.Material=Enum.Material.Neon
                    local BG,BP=attachPhysics(pp,20000,300)
                    local t={BG=BG,BP=BP,Pallet=pp,Model=toy,seed=i}
                    AnimN3Points[i].assignedToy=t; table.insert(AnimN3AssignedToys,t)
                end
            end
        end
        startAnimN3Loop()
        OrionLib:MakeNotification({Name="N3開始",Content="数:"..#AnimN3AssignedToys,Time=3})
    else
        stopAnimN3Loop()
        OrionLib:MakeNotification({Name="N3停止",Content="解除",Time=2})
    end
end

-- ====================================================================
-- TPWalk / シンプルESP
-- ====================================================================
local function enableTPWalk2()
    if TPWalkConnection2 then TPWalkConnection2:Disconnect();TPWalkConnection2=nil end
    local char=LocalPlayer.Character
    if char then
        local hum=char:FindFirstChildOfClass("Humanoid")
        if hum then OriginalWalkSpeed2=hum.WalkSpeed end
    end
    TPWalkConnection2=RunService.RenderStepped:Connect(function()
        if not UtilityConfig.TPWalk or not LocalPlayer.Character then return end
        local hum=LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        local hrp=LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if hum and hrp then
            local md=hum.MoveDirection
            if md.Magnitude>0 then
                hrp.AssemblyLinearVelocity=Vector3.new(md.X*UtilityConfig.TPWalkSpeed,hrp.AssemblyLinearVelocity.Y,md.Z*UtilityConfig.TPWalkSpeed)
            else
                local mv=Vector3.new(0,hrp.AssemblyLinearVelocity.Y,0)
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then mv=mv+hrp.CFrame.LookVector*UtilityConfig.TPWalkSpeed end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then mv=mv-hrp.CFrame.LookVector*UtilityConfig.TPWalkSpeed end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then mv=mv-hrp.CFrame.RightVector*UtilityConfig.TPWalkSpeed end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then mv=mv+hrp.CFrame.RightVector*UtilityConfig.TPWalkSpeed end
                hrp.AssemblyLinearVelocity=mv
            end
        end
    end)
end

local function disableTPWalk2()
    if TPWalkConnection2 then TPWalkConnection2:Disconnect();TPWalkConnection2=nil end
    local char=LocalPlayer.Character
    if char then
        local hum=char:FindFirstChildOfClass("Humanoid")
        if hum then hum.WalkSpeed=OriginalWalkSpeed2 end
    end
end

local function enableSimpleESP()
    if SimpleESPConnection then SimpleESPConnection:Disconnect();SimpleESPConnection=nil end
    for _,l in pairs(SimpleESPLabels) do if l then l:Destroy() end end
    SimpleESPLabels={}
    SimpleESPConnection=RunService.RenderStepped:Connect(function()
        if not UtilityConfig.SimpleESP then return end
        for _,plr in ipairs(Players:GetPlayers()) do
            if plr~=LocalPlayer and plr.Character then
                local hrp=plr.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    if not SimpleESPLabels[plr] then
                        local bg=Instance.new("BillboardGui"); bg.Name="SESP_"..plr.Name
                        bg.Adornee=hrp; bg.AlwaysOnTop=true
                        bg.Size=UDim2.new(0,200,0,50); bg.StudsOffset=Vector3.new(0,3,0)
                        local tl=Instance.new("TextLabel"); tl.Size=UDim2.new(1,0,1,0)
                        tl.BackgroundTransparency=1; tl.TextColor3=Color3.fromRGB(255,105,180)
                        tl.TextStrokeTransparency=0; tl.TextStrokeColor3=Color3.new(0,0,0)
                        tl.Font=Enum.Font.SourceSansBold; tl.TextSize=20; tl.Parent=bg
                        bg.Parent=hrp; SimpleESPLabels[plr]=bg
                    end
                    local dist=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and
                        (hrp.Position-LocalPlayer.Character.HumanoidRootPart.Position).Magnitude or 0
                    local lbl=SimpleESPLabels[plr]
                    if lbl then
                        local tl=lbl:FindFirstChildOfClass("TextLabel")
                        if tl then tl.Text=string.format("%s\n[%.0fm]",plr.DisplayName,dist) end
                    end
                end
            end
        end
        for plr,lbl in pairs(SimpleESPLabels) do
            if not Players:FindFirstChild(plr.Name) then lbl:Destroy(); SimpleESPLabels[plr]=nil end
        end
    end)
end

local function disableSimpleESP()
    if SimpleESPConnection then SimpleESPConnection:Disconnect();SimpleESPConnection=nil end
    for _,l in pairs(SimpleESPLabels) do if l then l:Destroy() end end
    SimpleESPLabels={}
end

local function executeScript(url, scriptName)
    local ok,res=pcall(function() return loadstring(game:HttpGet(url))() end)
    OrionLib:MakeNotification({Name=scriptName,Content=ok and "読み込み成功！" or "失敗: "..tostring(res),Time=ok and 3 or 5})
end

local function copyToClipboard(text, name)
    setclipboard(text)
    OrionLib:MakeNotification({Name=name,Content="クリップボードにコピーしました",Time=2})
end

local Window = OrionLib:MakeWindow({
    Name = "Tsukuyomihub やおよろー！",
    SaveConfig = true,
    ConfigFolder = "Tsukuyomihub",
    IntroEnabled = true,
    IntroText = "Tsukuyomihub やおよろー！",
    IntroIcon = "rbxassetid://118739401073899",
    Icon = "rbxassetid://118739401073899",
    FreeMouse = true,
    KeyToOpenWindow = "M"
})

local GrabTab = Window:MakeTab({Name = "グラブ", Icon =  "rbxassetid://130703864968637", PremiumOnly = false})
local BlobmanTab = Window:MakeTab({Name = "ブロブマン", Icon =  "rbxassetid://7733916988", PremiumOnly = false})
local AntiTab = Window:MakeTab({Name = "防衛", Icon =  "rbxassetid://10734951847", PremiumOnly = false})
local PlayerTab = Window:MakeTab({Name = "ローカルプレイヤー", Icon =  "rbxassetid://11632434473", PremiumOnly = false})
local ESPTab = Window:MakeTab({Name = "ESP", Icon =  "rbxassetid://7733774602", PremiumOnly = false})

ESPTab:AddToggle({
    Name = "シンプルESP",
    Default = false,
    Callback = function(v)
        UtilityConfig.SimpleESP = v
        if v then enableSimpleESP() else disableSimpleESP() end
    end
})

ESPTab:AddSlider({
    Name = "視野角 (FOV)",
    Min = -5, Max = 180, Default = 70, Increment = 1,
    Callback = function(v)
        UtilityConfig.FOV2 = v
        Camera.FieldOfView = v
    end
})

ESPTab:AddButton({
    Name = "FOVリセット",
    Callback = function()
        Camera.FieldOfView = UtilityConfig.OriginalFOV2
        UtilityConfig.FOV2 = UtilityConfig.OriginalFOV2
    end
})

ESPTab:AddSection({Name = "Hokuto Hub ESP"})

ESPTab:AddButton({
    Name = "Hokuto Hub 起動 (RShift でGUI表示)",
    Callback = function()
        task.spawn(function()
            local _hPlayers = game:GetService("Players")
            local _hRun = game:GetService("RunService")
            local _hUIS = game:GetService("UserInputService")
            local _hWS = game:GetService("Workspace")
            local _hLP = _hPlayers.LocalPlayer
            local _hCam = _hWS.CurrentCamera
            local _hSettings = {
                toggleKey = Enum.KeyCode.RightShift,
                guiColor = Color3.fromRGB(255,105,180),
                textColor = Color3.fromRGB(255,255,255),
                backgroundColor = Color3.fromRGB(20,20,20),
                lineColor = Color3.fromRGB(255,165,0),
                transparency = 0.1
            }
            local _hAllGUI = false
            local _hESPOn = false
            local _hAimbotOn = false
            local _hAimrookOn = false
            local _hSelectedTarget = nil
            local _hAutoTarget = nil
            local _hTrackMode = "CAMERA"
            local _hESPLabels = {}
            local _hLines = {}
            local _hDrawings = {}
            local _hContainer = nil
            local _hESPFrame = nil
            local _hSystemFrame = nil
            local _hLogFrame = nil
            local _hGuiConns = {}
            local _hLastAimTime = 0
            local _hAimCD = 0.016

            local function _hNewDrawing(t)
                local d = Drawing.new(t)
                table.insert(_hDrawings, d)
                return d
            end
            local function _hClearDrawings()
                for _,d in ipairs(_hDrawings) do pcall(function() d:Remove() end) end
                _hDrawings = {}
            end
            local function _hMakeDrag(frame, bar)
                _hGuiConns[frame] = _hGuiConns[frame] or {}
                local drag, ds, sp = false, nil, nil
                table.insert(_hGuiConns[frame], bar.InputBegan:Connect(function(inp)
                    if inp.UserInputType == Enum.UserInputType.MouseButton1 or inp.UserInputType == Enum.UserInputType.Touch then
                        drag = true; ds = inp.Position
                        sp = Vector2.new(frame.Position.X.Offset, frame.Position.Y.Offset)
                        table.insert(_hGuiConns[frame], inp.Changed:Connect(function()
                            if inp.UserInputState == Enum.UserInputState.End then drag = false end
                        end))
                    end
                end))
                table.insert(_hGuiConns[frame], _hUIS.InputChanged:Connect(function(inp)
                    if drag and (inp.UserInputType == Enum.UserInputType.MouseMovement or inp.UserInputType == Enum.UserInputType.Touch) then
                        local delta = inp.Position - ds
                        local nx = math.clamp(sp.X+delta.X, 0, _hCam.ViewportSize.X-frame.AbsoluteSize.X)
                        local ny = math.clamp(sp.Y+delta.Y, 0, _hCam.ViewportSize.Y-frame.AbsoluteSize.Y)
                        frame.Position = UDim2.new(0,nx,0,ny)
                    end
                end))
            end
            local function _hMakeFrame(w,h,x,y,title)
                local f = Instance.new("Frame")
                f.Size = UDim2.new(0,w,0,h); f.Position = UDim2.new(0,x,0,y)
                f.BackgroundColor3 = _hSettings.backgroundColor
                f.BackgroundTransparency = _hSettings.transparency
                f.BorderSizePixel = 2; f.BorderColor3 = _hSettings.guiColor
                f.Visible = false; f.Parent = _hContainer
                local bar = Instance.new("TextButton")
                bar.Text = title; bar.Size = UDim2.new(1,0,0,25)
                bar.BackgroundColor3 = _hSettings.guiColor; bar.BackgroundTransparency = 0.2
                bar.TextColor3 = _hSettings.textColor; bar.Font = Enum.Font.GothamBold
                bar.TextSize = 13; bar.AutoButtonColor = false; bar.Parent = f
                _hMakeDrag(f, bar)
                return f, bar
            end
            local function _hBtn(parent, text, yPos, w, xOff)
                local b = Instance.new("TextButton")
                b.Text = text; b.Size = UDim2.new(w or 0.9,0,0,25)
                b.Position = UDim2.new(xOff or 0.05,0,0,yPos)
                b.BackgroundColor3 = _hSettings.guiColor; b.BackgroundTransparency = 0.3
                b.TextColor3 = _hSettings.textColor; b.Font = Enum.Font.GothamBold
                b.TextSize = 12; b.Parent = parent
                return b
            end

            _hContainer = Instance.new("ScreenGui")
            _hContainer.Name = "HokutoHub"; _hContainer.DisplayOrder = 1000
            _hContainer.ResetOnSpawn = false
            _hContainer.Parent = game:GetService("CoreGui") or _hLP:WaitForChild("PlayerGui")

            local _hEF, _ = _hMakeFrame(200,220,10,10,"ESP Control")
            local _bESP = _hBtn(_hEF,"ESP: OFF",30)
            local _bAimbot = _hBtn(_hEF,"Aimbot: OFF",60)
            local _bAimrook = _hBtn(_hEF,"Aimrook: OFF",90)
            local _bTarget = _hBtn(_hEF,"Target: NONE",120)
            local _bClear = _hBtn(_hEF,"Clear Target",150)
            local _bTrack = _hBtn(_hEF,"Track: CAMERA",180)
            _hESPFrame = _hEF

            local _hSF, _ = _hMakeFrame(180,120,220,10,"System Info")
            _hSystemFrame = _hSF

            local function _hUpdateESP(plr)
                if plr == _hLP or not plr.Character then return end
                local hrp = plr.Character:FindFirstChild("HumanoidRootPart")
                local hum = plr.Character:FindFirstChildOfClass("Humanoid")
                if not hrp or not hum then
                    local d = _hESPLabels[plr]; if d then for _,v in pairs(d) do v.Visible=false end end
                    local l = _hLines[plr]; if l then l.Visible=false end; return
                end
                local pos, onScreen = _hCam:WorldToViewportPoint(hrp.Position)
                local dist = (hrp.Position - _hCam.CFrame.Position).Magnitude
                local sc = Vector2.new(_hCam.ViewportSize.X/2, _hCam.ViewportSize.Y/2)
                if not _hESPLabels[plr] then
                    local name = _hNewDrawing("Text"); name.Color=_hSettings.guiColor; name.Size=14
                    name.Outline=true; name.OutlineColor=Color3.new(0,0,0); name.Font=Drawing.Fonts.UI
                    local box = _hNewDrawing("Square"); box.Color=_hSettings.guiColor; box.Thickness=2; box.Filled=false
                    local line = _hNewDrawing("Line"); line.Color=_hSettings.lineColor; line.Thickness=1
                    _hESPLabels[plr]={name=name,box=box}; _hLines[plr]=line
                end
                local d = _hESPLabels[plr]; local line = _hLines[plr]
                if line and _hESPOn then
                    if onScreen then line.From=sc; line.To=Vector2.new(pos.X,pos.Y)
                    else
                        local dir=(Vector2.new(pos.X,pos.Y)-sc).Unit
                        line.From=sc; line.To=sc+dir*500
                    end
                    line.Visible=true
                elseif line then line.Visible=false end
                if onScreen then
                    local head=plr.Character:FindFirstChild("Head"); local torso=plr.Character:FindFirstChild("UpperTorso") or plr.Character:FindFirstChild("Torso")
                    if head and torso then
                        local hp=_hCam:WorldToViewportPoint(head.Position)
                        local ph=math.abs(hp.Y-pos.Y)*2.5; local pw=ph*0.5
                        d.box.Size=Vector2.new(pw,ph); d.box.Position=Vector2.new(pos.X-pw/2,pos.Y-ph/2); d.box.Visible=_hESPOn
                        d.name.Position=Vector2.new(pos.X,pos.Y-ph/2-18); d.name.Text=plr.Name; d.name.Visible=_hESPOn
                    end
                else
                    for _,v in pairs(d) do v.Visible=false end
                end
            end

            local function _hAdjustCharOrientation(tpos)
                local char = _hLP.Character; if not char then return end
                local hrp = char:FindFirstChild("HumanoidRootPart"); if not hrp then return end
                if _hTrackMode == "CHARACTER" then
                    local dir = (tpos-hrp.Position).Unit
                    hrp.CFrame = CFrame.new(hrp.Position, hrp.Position+Vector3.new(dir.X,0,dir.Z))
                end
            end

            local function _hUpdateAimbot()
                if not _hAimbotOn then _hAutoTarget=nil; return end
                local cur = tick(); if cur-_hLastAimTime<_hAimCD then return end
                local char=_hLP.Character; local root=char and char:FindFirstChild("HumanoidRootPart"); if not root then return end
                local best,bestPart,closest=nil,nil,math.huge
                for _,plr in pairs(_hPlayers:GetPlayers()) do
                    if plr~=_hLP and plr.Character then
                        local hum=plr.Character:FindFirstChildOfClass("Humanoid")
                        if hum and hum.Health>0 then
                            local part=plr.Character:FindFirstChild("Head") or plr.Character:FindFirstChild("HumanoidRootPart")
                            if part then
                                local dist=(root.Position-part.Position).Magnitude
                                if dist<closest then closest=dist; best=plr; bestPart=part end
                            end
                        end
                    end
                end
                if best and bestPart then
                    _hAutoTarget=best
                    local cf=_hCam.CFrame
                    _hCam.CFrame=cf:Lerp(CFrame.lookAt(cf.Position,bestPart.Position),0.95)
                    _hAdjustCharOrientation(bestPart.Position)
                    _hLastAimTime=cur
                end
            end

            local function _hUpdateAimrook()
                if not _hAimrookOn or not _hSelectedTarget then return end
                local cur=tick(); if cur-_hLastAimTime<_hAimCD then return end
                if not _hSelectedTarget.Parent then _hSelectedTarget=nil; return end
                local tc=_hSelectedTarget.Character; if not tc then return end
                local tp=tc:FindFirstChild("Head") or tc:FindFirstChild("HumanoidRootPart"); if not tp then return end
                local cf=_hCam.CFrame
                local tgt=tp.Position
                local dir=(tgt-cf.Position).Unit
                _hCam.CFrame=CFrame.new(cf.Position, cf.Position+dir)
                _hAdjustCharOrientation(tgt)
                _hLastAimTime=cur
            end

            _bESP.MouseButton1Click:Connect(function()
                _hESPOn=not _hESPOn; _bESP.Text="ESP: "..(_hESPOn and "ON" or "OFF")
                _bESP.BackgroundTransparency=_hESPOn and 0.1 or 0.3
            end)
            _bAimbot.MouseButton1Click:Connect(function()
                _hAimbotOn=not _hAimbotOn; _bAimbot.Text="Aimbot: "..(_hAimbotOn and "ON" or "OFF")
                _bAimbot.BackgroundTransparency=_hAimbotOn and 0.1 or 0.3
                if _hAimbotOn then _hAimrookOn=false; _bAimrook.Text="Aimrook: OFF"; _bAimrook.BackgroundTransparency=0.3 end
            end)
            _bAimrook.MouseButton1Click:Connect(function()
                _hAimrookOn=not _hAimrookOn; _bAimrook.Text="Aimrook: "..(_hAimrookOn and "ON" or "OFF")
                _bAimrook.BackgroundTransparency=_hAimrookOn and 0.1 or 0.3
                if _hAimrookOn then _hAimbotOn=false; _bAimbot.Text="Aimbot: OFF"; _bAimbot.BackgroundTransparency=0.3 end
            end)
            _bTarget.MouseButton1Click:Connect(function()
                local menu=Instance.new("ScreenGui"); menu.Name="HHTargetMenu"; menu.DisplayOrder=1001
                menu.Parent=game:GetService("CoreGui") or _hLP:WaitForChild("PlayerGui")
                local frame=Instance.new("Frame"); frame.Size=UDim2.new(0,200,0,250)
                frame.Position=UDim2.new(0.5,-100,0.5,-125)
                frame.BackgroundColor3=_hSettings.backgroundColor; frame.BackgroundTransparency=0.05
                frame.BorderSizePixel=2; frame.BorderColor3=_hSettings.guiColor; frame.Parent=menu
                local cl=Instance.new("TextButton"); cl.Text="✕ 閉じる"; cl.Size=UDim2.new(1,0,0,30)
                cl.BackgroundColor3=_hSettings.guiColor; cl.TextColor3=_hSettings.textColor
                cl.Font=Enum.Font.GothamBold; cl.TextSize=13; cl.Parent=frame
                cl.MouseButton1Click:Connect(function() menu:Destroy() end)
                local y=35
                for _,plr in pairs(_hPlayers:GetPlayers()) do
                    if plr~=_hLP then
                        local pb=Instance.new("TextButton"); pb.Text=plr.Name
                        pb.Size=UDim2.new(0.9,0,0,28); pb.Position=UDim2.new(0.05,0,0,y)
                        pb.BackgroundColor3=Color3.fromRGB(50,50,50); pb.BackgroundTransparency=0.3
                        pb.TextColor3=_hSettings.textColor; pb.Font=Enum.Font.Gotham; pb.TextSize=12; pb.Parent=frame
                        pb.MouseButton1Click:Connect(function()
                            _hSelectedTarget=plr; _bTarget.Text="Target: "..plr.Name; menu:Destroy()
                        end)
                        y=y+32
                    end
                end
                frame.Size=UDim2.new(0,200,0,math.min(y+5,300))
            end)
            _bClear.MouseButton1Click:Connect(function()
                _hSelectedTarget=nil; _bTarget.Text="Target: NONE"
            end)
            _bTrack.MouseButton1Click:Connect(function()
                _hTrackMode=(_hTrackMode=="CAMERA") and "CHARACTER" or "CAMERA"
                _bTrack.Text="Track: ".._hTrackMode
            end)
            _hUIS.InputBegan:Connect(function(inp, gp)
                if gp then return end
                if inp.KeyCode==_hSettings.toggleKey then
                    _hAllGUI=not _hAllGUI
                    _hEF.Visible=_hAllGUI; _hSF.Visible=_hAllGUI
                end
            end)
            for _,plr in ipairs(_hPlayers:GetPlayers()) do
                if plr~=_hLP then pcall(function() _hUpdateESP(plr) end) end
            end
            _hPlayers.PlayerAdded:Connect(function(plr) task.wait(1); pcall(function() _hUpdateESP(plr) end) end)
            _hPlayers.PlayerRemoving:Connect(function(plr)
                if _hESPLabels[plr] then for _,d in pairs(_hESPLabels[plr]) do pcall(function() d:Remove() end) end; _hESPLabels[plr]=nil end
                if _hLines[plr] then pcall(function() _hLines[plr]:Remove() end); _hLines[plr]=nil end
            end)
            _hRun.RenderStepped:Connect(function()
                for plr in pairs(_hESPLabels) do pcall(function() _hUpdateESP(plr) end) end
                if _hAimrookOn and _hSelectedTarget then _hUpdateAimrook()
                elseif _hAimbotOn then _hUpdateAimbot() end
            end)
            OrionLib:MakeNotification({Name="Hokuto Hub",Content="起動完了！RShiftでGUI表示",Time=4})
        end)
    end
})

local LineTab = Window:MakeTab({Name = "ライン", Icon =  "rbxassetid://7734022107", PremiumOnly = false})
local TeleportTab = Window:MakeTab({Name = "テレポート", Icon =  "rbxassetid://7734022107", PremiumOnly = false})
local NotifyTab = Window:MakeTab({Name = "通知", Icon = "rbxassetid://94612128913941", PremiumOnly = false})
local AuraTab = Window:MakeTab({Name = "オーラ", Icon =  "rbxassetid://7734022107", PremiumOnly = false})
local bindTab = Window:MakeTab({Name="バインド", Icon="rbxassetid://11710306232", PremiumOnly=false})
local ObjectTab = Window:MakeTab({Name="オブジェクト", Icon="rbxassetid://", PremiumOnly=false})
local AutoTab = Window:MakeTab({Name = "オート", Icon = "rbxassetid://7733916988", PremiumOnly = false})
local LoopTab = Window:MakeTab({Name = "ループ", Icon = "rbxassetid://7733916988", PremiumOnly = false})
local MiscTab = Window:MakeTab({Name = "その他", Icon = "rbxassetid://101768155599700", PremiumOnly = false})
local VisualTab = Window:MakeTab({Name = "ビジュアル", Icon = "rbxassetid://4483345998", PremiumOnly = false})
local WorldTab = Window:MakeTab({Name = "ワールド", Icon = "rbxassetid://4483345998", PremiumOnly = false})

GrabTab:AddToggle({
	Name = "ストレングス",
	Default = false,
	Callback = function(enabled)
		if enabled then
			Config.strengthConnection = workspace.ChildAdded:Connect(function(model)
				if model.Name == "GrabParts" then
					local grabPart = model:FindFirstChild("GrabPart")
					local weld = grabPart and grabPart:FindFirstChild("WeldConstraint")
					local partToImpulse = weld and weld.Part1

					if partToImpulse then
						local velocityObj = Instance.new("BodyVelocity")
						velocityObj.Parent = partToImpulse
						velocityObj.MaxForce = Vector3.zero

						model:GetPropertyChangedSignal("Parent"):Connect(function()
							if not model.Parent then
								local lastInput = service.UserInputService:GetLastInputType()
								if lastInput == Enum.UserInputType.MouseButton2 or lastInput == Enum.UserInputType.Touch then
									velocityObj.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
									velocityObj.Velocity = workspace.CurrentCamera.CFrame.LookVector * Config.nageru
									service.Debris:AddItem(velocityObj, 1)
								else
									velocityObj:Destroy()
								end
							end
						end)
					end
				end
			end)
		elseif Config.strengthConnection then
			Config.strengthConnection:Disconnect()
			Config.strengthConnection = nil
		end
	end
})

GrabTab:AddSlider({
	Name = "ストレングスパワー",
	Min = 300,
	Max = 10000,
	Default = 400,
	Increment = 1,
	Callback = function(value)
		Config.nageru = value
	end
})

GrabTab:AddToggle({
	Name = "キルグラブ",
	Default = false,
	Callback = function(v)
        Config.killGrabToggle = v
	end
})

GrabTab:AddToggle({
	Name = "デスグラブ",
	Default = false,
	Callback = function(Value)
		Config.DeathGrabToggle = Value
	end    
})

GrabTab:AddToggle({
	Name = "スカイグラブ",
	Default = false,
	Callback = function(v)
        Config.skyGrabToggle = v
	end
})

GrabTab:AddToggle({
    Name = "ラグドールグラブ",
    Default = false,
    Callback = function(Value)
        Config.RagdollGrab = Value

        for i, v in pairs(Config.Cons) do
            if string.find(i, "RagdollGrab") then
                v:Disconnect()
                Config.Cons[i] = nil
            end
        end

        if Value then
            local ragdoll = SpawnedInToys:FindFirstChild("Ragdoll")

            if not ragdoll then
                local head = localPlayer.Character:FindFirstChild("Head") or localPlayer.Character:WaitForChild("Head", 3)
                if not head then return end

                local pallet = SPNTOY("PalletLightBrown", head.CFrame)
                if not pallet then return end

                local sound = pallet:WaitForChild("SoundPart", 3)
                if not sound then return end

                while sound and (not sound:FindFirstChild("PartOwner") or sound.PartOwner.Value ~= localPlayer.Name) do
                    SetNetworkOwner:FireServer(sound, sound.CFrame)
                    task.wait()
                end

                local BV = Instance.new("BodyVelocity")
                BV.Parent = sound
                BV.MaxForce = Vector3.new(0, math.huge, 0)
                BV.Velocity = Vector3.new(0, 1000, 0)

                for _, v in pairs(pallet:GetChildren()) do
                    if v:IsA("BasePart") then
                        v.Transparency = 1
                        v.Size = Vector3.new(0.5, 0.5, 0.5)
                    end
                end

                pallet.Name = "Ragdoll"
                ragdoll = pallet
            end

            local sound = ragdoll:FindFirstChild("SoundPart")
            if not sound then return end

            Config.Cons["RagdollGrab"] = Workspace.ChildAdded:Connect(function(part)
                if not Config.RagdollGrab then return end

                if part.Name == "GrabParts" then
                    local gp = part:FindFirstChild("GrabPart")
                    if not gp then return end

                    local weld = gp:FindFirstChild("WeldConstraint")
                    if not weld then return end

                    local target = weld.Part1
                    if not target then return end

                    local char = target.Parent
                    local head = char and char:FindFirstChild("Head")

                    if head then
                        firetouchinterest(sound, head, 0)
                    end
                end
            end)
        else
            local ragdoll = SpawnedInToys:FindFirstChild("Ragdoll")
            if ragdoll then
                for _, v in pairs(ragdoll:GetDescendants()) do
                    if v.Name == "PartOwner" then
                        DestroyGrabLine:FireServer(v.Parent)
                    end
                end
            end
        end
    end
})

GrabTab:AddToggle({
	Name = "スピングラブ",
	Default = false,
	Callback = function(v)
        Config.spinGrabToggle = v
	end
})

GrabTab:AddToggle({
	Name = "テレポートグラブ (上)",
	Default = false,
	Callback = function(v)
        Config.teleportUpToggle = v
	end
})

GrabTab:AddToggle({
	Name = "テレポートグラブ (下)",
	Default = false,
	Callback = function(v)
        Config.teleportDownToggle = v
	end
})

GrabTab:AddToggle({
	Name = "カオスグラブ",
	Default = false,
	Callback = function(v)
        Config.chaosGrabToggle = v
	end
})

GrabTab:AddToggle({
	Name = "超音速グラブ",
	Default = false,
	Callback = function(v)
        Config.supersonicGrabToggle = v
	end
})

GrabTab:AddToggle({
	Name = "フリーズグラブ",
	Default = false,
	Callback = function(v)
        Config.freezeGrabToggle = v
	end
})

GrabTab:AddToggle({
	Name = "ノークリップグラブ",
	Default = false,
	Callback = function(v)
        Config.noclipGrabToggle = v
	end
})

GrabTab:AddToggle({
    Name = "アンカーグラブ",
    Default = false,
    Callback = function(Value)
        Config.anchorGrabToggle = Value
    end    
})

GrabTab:AddToggle({
    Name = "無重力グラブ",
    Default = false,
    Callback = function(Value)
        Config.masslessGrabToggle = Value
    end
})

local TargetDropdown_blob = BlobmanTab:AddDropdown({
    Name = "ターゲット選択",
    Default = "",
    Options = getPlayerList(),
    Callback = function(Value) Config.selectedTarget = Value end
})

local function updateTargetDropdown()
    TargetDropdown_blob:Refresh(getPlayerList(), true)
end
service.Players.PlayerAdded:Connect(updateTargetDropdown)
service.Players.PlayerRemoving:Connect(updateTargetDropdown)

BlobmanTab:AddSection({
	Name = "テスト版です。ブロブグラブが動かない場合は何度か試してください。"
})

BlobmanTab:AddButton({
    Name = "キック",
    Callback = function()
        task.spawn(function()
            local targetPlr = checkTarget()
            if not targetPlr then return end
            
            local target = targetPlr.Character and targetPlr.Character:FindFirstChild("HumanoidRootPart")
            local myRoot = GetRoot()
            if not target or not myRoot then return end
            
            local blob = ensureBlob()
            if not blob then return end
            
            local side = (math.random() >= 0.5) and "Left" or "Right"
            local pos = myRoot.CFrame
            
            task.wait(0.1)
            myRoot.CFrame = target.CFrame
            task.wait(0.08)
            
            blobGrab(blob, myRoot, side)
            task.wait(0.08)
            SetNetworkOwner:FireServer(target, myRoot.CFrame)
            task.wait(0.08)
            target.CFrame = target.CFrame + Vector3.new(0, Config.FloatAmount, 0)
            task.wait(0.08)
            DestroyGrabLine:FireServer(target)
            task.wait(0.08)
            blobGrab(blob, target, side)
            task.wait(0.08)
            blobDrop(blob, target, side)
            task.wait(0.08)
            DestroyGrabLine:FireServer(target)
            
            task.wait(0.1)
            myRoot.CFrame = pos
        end)
    end
})

BlobmanTab:AddButton({
    Name = "ブリング",
    Callback = function()
        task.spawn(function()
            local targetPlr = checkTarget()
            if not targetPlr then return end
            
            local target = targetPlr.Character and targetPlr.Character:FindFirstChild("HumanoidRootPart")
            local myRoot = GetRoot()
            if not target or not myRoot then return end
            
            local blob = ensureBlob()
            if not blob then return end
            
            local side = (math.random() >= 0.5) and "Left" or "Right"
            local myPos = myRoot.CFrame
            
            SetNetworkOwner:FireServer(target, myRoot.CFrame)
            task.wait(0.15)
            myRoot.CFrame = target.CFrame
            task.wait(0.15)
            blobGrab(blob, target, side)
            task.wait(0.2)
            myRoot.CFrame = myPos
        end)
    end
})

BlobmanTab:AddButton({
    Name = "スライド",
    Callback = function()
        task.spawn(function()
            local targetPlr = checkTarget()
            if not targetPlr then return end
            
            local target = targetPlr.Character and targetPlr.Character:FindFirstChild("HumanoidRootPart")
            local myRoot = GetRoot()
            if not target or not myRoot then return end
            
            local blob = ensureBlob()
            if not blob then return end
            
            local side = (math.random() >= 0.5) and "Left" or "Right"
            local pos = myRoot.CFrame
            
            SetNetworkOwner:FireServer(target, myRoot.CFrame)
            task.wait(0.08)
            myRoot.CFrame = target.CFrame
            task.wait(0.08)
            blobGrab(blob, myRoot, side)
            task.wait(0.08)
            SetNetworkOwner:FireServer(target, myRoot.CFrame)
            task.wait(0.08)
            target.CFrame = target.CFrame + Vector3.new(0, 2, 0)
            task.wait(0.08)
            DestroyGrabLine:FireServer(target)
            task.wait(0.08)
            blobGrab(blob, target, side)
            task.wait(0.08)
            blobDrop(blob, target, side)
            task.wait(0.08)
            DestroyGrabLine:FireServer(target)
            task.wait(0.08)
            myRoot.CFrame = pos
        end)
    end
})

BlobmanTab:AddSection({
	Name = "オーラ"
})

BlobmanTab:AddToggle({
    Name = "キルオーラ",
    Default = false,
    Callback = function(Value)
        Config.Running = Value
        
        local function spawnBlobmanIfNeeded()
            local inv = getInv()
            if not inv then return nil end
            
            local existing = inv:FindFirstChild(Config.ToyName)
            if existing then return existing end
            
            local char = GetCharacter()
            if char and char:FindFirstChild("Head") then
                pcall(function()
                    SPNTOY(Config.ToyName, char.Head.CFrame)
                end)
            end
            
            return inv:WaitForChild(Config.ToyName, 5)
        end

        local function sitOnBlobman(blobman)
            if not blobman then return end
            local seat = blobman:FindFirstChildWhichIsA("VehicleSeat", true)
            local hum = getLocalHum()
            if seat and hum and not seat.Occupant then
                task.wait(0.1)
                pcall(function() seat:Sit(hum) end)
            end
        end

        local function runKillAura()
            local hpToggle = false
            while Config.Running do
                task.wait(0.01)
                local _, hrp = getCharInfo()
                if not hrp then continue end
                
                local blobman = spawnBlobmanIfNeeded()
                if not blobman then continue end
                
                sitOnBlobman(blobman)
                
                local scriptFolder = blobman:FindFirstChild("BlobmanSeatAndOwnerScript")
                local detector = blobman:FindFirstChild("LeftDetector")
                local weld = detector and detector:FindFirstChild("LeftWeld")
                
                if scriptFolder and detector and weld then
                    local CreatureGrab = scriptFolder:FindFirstChild("CreatureGrab")
                    local CreatureRelease = scriptFolder:FindFirstChild("CreatureRelease")
                    local CreatureDrop = scriptFolder:FindFirstChild("CreatureDrop")
                    
                    if CreatureGrab and CreatureRelease and CreatureDrop then
                        hpToggle = not hpToggle
                        for _, plr in ipairs(service.Players:GetPlayers()) do
                            if not Config.Running then break end
                            
                            if plr ~= localPlayer then
                                local targetChar = plr.Character
                                local thrp = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
                                local thum = targetChar and targetChar:FindFirstChildOfClass("Humanoid")
                                
                                if thrp and thum and (hrp.Position - thrp.Position).Magnitude <= Config.Range then
                                    pcall(function()
                                        CreatureGrab:FireServer(detector, thrp, weld)
                                        CreatureRelease:FireServer(weld, thrp)
                                        CreatureDrop:FireServer(weld, thrp)
                                        
                                        thum.Health = hpToggle and 0 or 100
                                    end)
                                end
                            end
                        end
                    end
                end
            end
        end

        if Value then
            task.spawn(runKillAura)
        end
    end
})

BlobmanTab:AddSection({
	Name = "設定"
})

BlobmanTab:AddToggle({
    Name = "ブロブマンRGB (テスト)",
    Default = false,
    Callback = function(Value)
        Config.AIRNJIAD = Config.AIRNJIAD or {}
        
        if Config.BLESP == Value then return end
        Config.BLESP = Value

        local function BLBOESP(model)
            if not Config.BLESP then return end
            if model:FindFirstChild("BlobmanESP") then return end

            local highlight = Instance.new("Highlight")
            highlight.Name = "BlobmanESP"
            highlight.FillTransparency = 0.3
            highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
            highlight.Adornee = model
            highlight.Parent = model

            table.insert(Config.AIRNJIAD, highlight)
        end

        local function isValidBlobman(obj)
            return obj:IsA("Model")
                and obj.Name == "CreatureBlobman"
                and obj.Parent
                and obj.Parent.Name:find("SpawnedInToys")
        end

        local function SOKDOI()
            if Config.RDESDG then return end
            Config.RDESDG = game:GetService("RunService").RenderStepped:Connect(function()
                local hue = tick() % 5 / 5
                for i = #Config.AIRNJIAD, 1, -1 do
                    local hl = Config.AIRNJIAD[i]
                    if not hl or not hl.Parent then
                        table.remove(Config.AIRNJIAD, i)
                    else
                        hl.FillColor = Color3.fromHSV(hue, 1, 1)
                    end
                end
            end)
        end

        local function GASGS()
            if Config.RDESDG then
                Config.RDESDG:Disconnect()
                Config.RDESDG = nil
            end
        end

        local function GWGEVwgwg()
            for i = #Config.AIRNJIAD, 1, -1 do
                local hl = Config.AIRNJIAD[i]
                if hl then
                    hl:Destroy()
                end
                table.remove(Config.AIRNJIAD, i)
            end
        end

        local function scan()
            for _, v in ipairs(workspace:GetDescendants()) do
                if isValidBlobman(v) then
                    BLBOESP(v)
                end
            end
        end

        if Value then
            if Config.BlobmanConnection then 
                Config.BlobmanConnection:Disconnect() 
            end
            
            Config.BlobmanConnection = workspace.DescendantAdded:Connect(function(obj)
                if isValidBlobman(obj) then
                    task.wait(0.2)
                    BLBOESP(obj)
                end
            end)

            scan()
            SOKDOI()
        else
            if Config.BlobmanConnection then
                Config.BlobmanConnection:Disconnect()
                Config.BlobmanConnection = nil
            end
            GASGS()
            GWGEVwgwg()
        end
    end
})

BlobmanTab:AddToggle({
    Name = "ノークリップ",
    Default = false,
    Callback = function(v)
        Config.BlobNoclip = v
        
        if v then
            task.spawn(function()
                while Config.BlobNoclip do
                    if SpawnedInToys then
                        local count = 0
                        for _, item in ipairs(SpawnedInToys:GetChildren()) do
                            if item.Name == "CreatureBlobman" then
                                if count < 4 then
                                    count = count + 1
                                    for _, descendant in ipairs(item:GetDescendants()) do
                                        if descendant:IsA("BasePart") then
                                            descendant.CanQuery = false
                                            descendant.CanTouch = false
                                            descendant.CanCollide = false
                                        end
                                    end
                                else
                                    break
                                end
                            end
                        end
                    end
                    task.wait()
                end
            end)
        else
            if SpawnedInToys then
                for _, item in ipairs(SpawnedInToys:GetChildren()) do
                    if item.Name == "CreatureBlobman" then
                        for _, descendant in ipairs(item:GetDescendants()) do
                            if descendant:IsA("BasePart") then
                                descendant.CanQuery = true
                                descendant.CanTouch = true
                                descendant.CanCollide = true
                            end
                        end
                    end
                end
            end
        end
    end
})

BlobmanTab:AddToggle({
    Name = "歩行速度",
    Default = false,
    Callback = function(Value)
        Config.BlobSpeedToggle = Value
        
        if Value then
            task.spawn(function()
                while Config.BlobSpeedToggle do
                    local char = localPlayer.Character
                    local hum = char and char:FindFirstChild("Humanoid")
                    
                    if hum then
                        local seat = hum.SeatPart
                        local isRiding = seat and seat.Parent and seat.Parent.Name == "CreatureBlobman"
                        
                        if isRiding then
                            local hrp = char:FindFirstChild("HumanoidRootPart")
                            if hrp then
                                local moveDir = hum.MoveDirection
                                hrp.CFrame = hrp.CFrame + moveDir * (Config.BlobSpeedValue / 10)
                            end
                        else
                            hum.WalkSpeed = 16
                        end
                    end
                    task.wait()
                end
                
                local char = localPlayer.Character
                if char and char:FindFirstChild("Humanoid") then
                    char.Humanoid.WalkSpeed = 16
                end
            end)
        end
    end
})

BlobmanTab:AddSlider({
    Name = "速度",
    Min = 16,
    Max = 300,
    Default = 16,
    Increment = 1,
    Callback = function(Value)
        Config.BlobSpeedValue = Value

        local char = localPlayer.Character
        local hum = char and char:FindFirstChild("Humanoid")
        if hum then
            local seat = hum.SeatPart
            local isRiding = seat and seat.Parent and seat.Parent.Name == "CreatureBlobman"
            
            if isRiding then
                hum.WalkSpeed = Value
            else
                hum.WalkSpeed = 16
            end
        end
    end
})

BlobmanTab:AddSlider({
    Name = "腰の高さ",
    Min = -500,
    Max = 500,
    Increment = 10,
    Default = 0,
    Callback = function(value)
        local function updateHipHeight(targetValue)
            if not SpawnedInToys then return end

            local creatureBlobmanCount = 0
            for _, item in ipairs(SpawnedInToys:GetChildren()) do
                if item.Name == "CreatureBlobman" then
                    if creatureBlobmanCount < 4 then
                        creatureBlobmanCount = creatureBlobmanCount + 1
                        for _, descendant in ipairs(item:GetDescendants()) do
                            if descendant:IsA("Humanoid") then
                                if Config.ORHHi == nil then
                                    Config.ORHHi = descendant.HipHeight
                                end
                                descendant.HipHeight = targetValue
                            end
                        end
                    else
                        break
                    end
                end
            end
        end

        updateHipHeight(value)
    end
})

BlobmanTab:AddSection({
	Name = "その他"
})

BlobmanTab:AddToggle({
    Name = "自動着席",
    Default = false,
    Callback = function(Value)
        Config.AutoBlobSit = Value
        if not Value and Config.blobSeat then
            local char = localPlayer.Character
            if char then
                local humanoid = char:FindFirstChild("Humanoid")
                if humanoid then
                    humanoid.Sit = false
                end
            end
            Config.blobSeat = nil
        end
    end
})

BlobmanTab:AddButton({
    Name = "ブロブマンを出す",
    Callback = function()
        local char = localPlayer.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        if not hrp then return end

        local function checkExisting()
            return SpawnedInToys and SpawnedInToys:FindFirstChild("CreatureBlobman")
        end
        if checkExisting() then return end

        local success, blob = pcall(function()
            return SpawnToyRemoteFunction:InvokeServer(
                "CreatureBlobman",
                hrp.CFrame,
                Vector3.zero
            )
        end)

        if success and blob and typeof(blob) == "Instance" then
            task.wait(0.1)
            if blob.Parent then
                blob:PivotTo(hrp.CFrame * CFrame.new(0, 0, -5))
            end
        end
    end
})

BlobmanTab:AddButton({
    Name = "ブロブマンを消す",
    Callback = function()
        local blob = SpawnedInToys and SpawnedInToys:FindFirstChild("CreatureBlobman")
        
        if not blob then return end

        DestroyToy:FireServer(blob)
    end
})

AntiTab:AddToggle({
    Name = "アンチグラブ",
    Default = false,
    Callback = function(value)
        Config.AntiGrabToggle = value
        
        if value then
            task.spawn(function()
                while Config.AntiGrabToggle do
                    pcall(function()
                        Struggle:FireServer(localPlayer)
                    end)
                    
                    Config.ragdollCount = 0
                    
                    task.wait(0.1)
                end
            end)

            local function AntiGrabT(character)
                local hrp = character:WaitForChild("HumanoidRootPart", 10)
                local hum = character:WaitForChild("Humanoid", 10)
                local head = character:WaitForChild("Head", 10)
                
                if not (hrp and hum and head) then return end

                head.ChildAdded:Connect(function(child)
                    if child.Name ~= "PartOwner" or not Config.AntiGrabToggle then return end
                    
                    hum.Sit = false
                    
                    if Config.ragdollCount < 100 then
                        RagdollRemote:FireServer(hrp, 0)
                        Config.ragdollCount = Config.ragdollCount + 1
                    end

                    if not Config.antiGrabProce then
                        Config.antiGrabProce = true
                        hrp.Anchored = true

                        local isHeld = localPlayer:WaitForChild("IsHeld", 5)
                        while Config.AntiGrabToggle and (not isHeld or not isHeld.Value) do 
                            task.wait() 
                        end

                        local renderConnection = game:GetService("RunService").RenderStepped:Connect(function()
                            hrp.CFrame += hum.MoveDirection * 0.3
                        end)

                        while Config.AntiGrabToggle and isHeld and isHeld.Value do 
                            task.wait() 
                        end
                        
                        renderConnection:Disconnect()
                        hrp.Anchored = false
                        Config.antiGrabProce = false
                    end
                end)
            end

            if localPlayer.Character then task.spawn(AntiGrabT, localPlayer.Character) end
            localPlayer.CharacterAdded:Connect(AntiGrabT)
        end
    end,
})

AntiTab:AddToggle({
    Name = "アンチ炎上",
    Default = false,
    Callback = function(enabled)
        if enabled then
            if not connection then
                connection = game:GetService("RunService").Heartbeat:Connect(function()
                    local char = localPlayer.Character
                    local hrp = char and char:FindFirstChild("HumanoidRootPart")
                    
                    if hrp then
                        local fireLight = hrp:FindFirstChild("FireLight")
                        local fireEmitter = hrp:FindFirstChild("FireParticleEmitter")

                        if fireLight or fireEmitter then
                            extPart.CFrame = hrp.CFrame
                        else
                            extPart.CFrame = CFrame.new(originalPos)
                        end
                    end
                end)
            end
        else
            if connection then
                connection:Disconnect()
                connection = nil
            end
            extPart.CFrame = CFrame.new(originalPos)
        end
    end
})

AntiTab:AddToggle({
    Name = "アンチ爆発",
    Default = false,
    Callback = function(state)
        local PlayerScripts = localPlayer:FindFirstChild("PlayerScripts")
        if PlayerScripts then
            local handler = PlayerScripts:FindFirstChild("ClientExoplosionHandler")
            if handler then
                handler.Enabled = not state
            end
        end
    end
})

AntiTab:AddToggle({
    Name = "アンチ [バナナ + 雪玉]",
    Default = false,
    Callback = function(Value)
        Config.antibananaSit = Value
        if Value then
            task.spawn(function()
                while Config.antibananaSit do
                    task.wait()
                    local char = GetCharacter()
                    local hum = char:FindFirstChildOfClass("Humanoid")
                    local hrp = char:FindFirstChild("HumanoidRootPart")
                    if hum and hrp and hum.Health > 0 then
                        hum.Sit = true
                        hum:ChangeState(Enum.HumanoidStateType.Running)
                        local vec = Camera.CFrame.LookVector
                        hrp.CFrame = CFrame.new(
                            hrp.Position,
                            hrp.Position + Vector3.new(vec.X, 0, vec.Z)
                        )
                        task.wait()
                        hum:ChangeState(Enum.HumanoidStateType.Running)
                    end
                end
            end)
        end
    end
})

AntiTab:AddToggle({
    Name = "アンチネットワークオーナー",
    Default = false, 
    Callback = function(Value)
        _G.AntiInputLag = Value
        if Value then
            task.spawn(function()
                local ToyName = "FoodHamburger"
                while _G.AntiInputLag do
                    local char, hrp = getCharInfo()
                    if not hrp then 
                        service.RunService.Heartbeat:Wait() 
                        continue 
                    end
                    local burger = SpawnedInToys and SpawnedInToys:FindFirstChild(ToyName)
                    if not burger and _G.AntiInputLag then
                        pcall(function() 
                            SpawnToyRemoteFunction:InvokeServer(ToyName, hrp.CFrame * CFrame.new(0, 5, 0), Vector3.new(0, 0, 0)) 
                        end)
                        continue
                    end
                    if burger and _G.AntiInputLag then
                        local holdPart = burger:FindFirstChild("HoldPart", true)
                        if holdPart then
                            local hRemote = holdPart:FindFirstChild("HoldItemRemoteFunction")
                            local dRemote = holdPart:FindFirstChild("DropItemRemoteFunction")
                            local hPlayer = holdPart:FindFirstChild("HoldingPlayer")
                            if hPlayer and hPlayer.Value and hPlayer.Value ~= LocalPlayer then
                                if dRemote then 
                                    pcall(function() 
                                        dRemote:InvokeServer(burger, hrp.CFrame * CFrame.new(0, 2000, 0), Vector3.new(0, 0, 0)) 
                                    end) 
                                end
                                burger:Destroy()
                            elseif hRemote and dRemote then
                                pcall(function()
                                    hRemote:InvokeServer(burger, char)
                                    dRemote:InvokeServer(burger, hrp.CFrame * CFrame.new(0, 2000, 0), Vector3.new(0, 0, 0))
                                end)
                            end
                        end
                    end
                end
            end)
        end
    end
})
AntiTab:AddToggle({
	Name = "アンチスティッキー",
	Default = false,
	Callback = function(Value)
		Config.antiStickyToggle = Value
		if localPlayer.PlayerScripts:FindFirstChild("StickyPartsTouchDetection") then
			localPlayer.PlayerScripts.StickyPartsTouchDetection.Disabled = Value
		end
	end,
})

AntiTab:AddToggle({
    Name = "アンチペイント",
    Default = false,
    Callback = function(state)
        local function setTouchQuery(s)
            local char = localPlayer.Character
            if char then
                for _, v in ipairs(char:GetChildren()) do
                    if v:IsA("BasePart") then
                        v.CanTouch = s
                        v.CanQuery = s
                    end
                end
            end
        end

        if state then
            for _, obj in ipairs(workspace:GetDescendants()) do
                if obj:IsA("BasePart") and obj.Name == "PaintPlayerPart" then
                    local clone = obj:Clone()
                    clone.Archivable = true
                    Config.paintPartsBackup[obj:GetDebugId()] = {
                        clone = clone,
                        parent = obj.Parent
                    }
                    obj:Destroy()
                end
            end

            local conn = workspace.DescendantAdded:Connect(function(obj)
                if obj:IsA("BasePart") and obj.Name == "PaintPlayerPart" then
                    task.defer(function()
                        if obj and obj.Parent then
                            local clone = obj:Clone()
                            clone.Archivable = true
                            Config.paintPartsBackup[obj:GetDebugId()] = {
                                clone = clone,
                                parent = obj.Parent
                            }
                            obj:Destroy()
                        end
                    end)
                end
            end)
            table.insert(Config.paintConnections, conn)

            setTouchQuery(false)
        else
            for _, conn in ipairs(Config.paintConnections) do
                if conn.Connected then conn:Disconnect() end
            end
            Config.paintConnections = {}

            for id, data in pairs(Config.paintPartsBackup) do
                if data.clone and data.parent then
                    data.clone.Parent = data.parent
                end
            end
            Config.paintPartsBackup = {}

            setTouchQuery(true)
        end
    end
})

AntiTab:AddToggle({
    Name = "アンチAFK",
    Default = false,
    Callback = function(Value)

        Config.antiAFKToggle = Value
        if not Value then return end

        localPlayer.Idled:Connect(function()
            if Config.antiAFKToggle then
                service.VirtualUser:CaptureController()
                service.VirtualUser:ClickButton2(Vector2.new())
            end
        end)

        task.spawn(function()
            while Config.antiAFKToggle do
                service.VirtualUser:CaptureController()
                service.VirtualUser:Button1Down(Vector2.new(0,0))
                task.wait(0.1)
                service.VirtualUser:Button1Up(Vector2.new(0,0))
                task.wait(600)
            end
        end)

    end
})

AntiTab:AddToggle({
    Name = "アンチボイド",
    Default = false,
    Callback = function(state)
        if state then
            if not Config.originalVoidHeight then
                Config.originalVoidHeight = workspace.FallenPartsDestroyHeight
            end
            workspace.FallenPartsDestroyHeight = -1e95
        else
            workspace.FallenPartsDestroyHeight = Config.originalVoidHeight or -500
        end
    end
})

AntiTab:AddToggle({
    Name = "アンチラグドール",
    Default = false,
    Callback = function(Value)
        _G.protectionEnabled = Value
        if not _G.connections then _G.connections = {} end

        local function clearConnections()
            for _, conn in ipairs(_G.connections) do
                if conn then conn:Disconnect() end
            end
            _G.connections = {}
        end

        local function protectHumanoid(humanoid)
            humanoid.BreakJointsOnDeath = false
            humanoid.AutoRotate = true
            humanoid.PlatformStand = false

            table.insert(_G.connections, humanoid.HealthChanged:Connect(function(health)
                if _G.protectionEnabled and health <= 0 then
                    humanoid.Health = 1
                end
            end))

            table.insert(_G.connections, humanoid:GetPropertyChangedSignal("AutoRotate"):Connect(function()
                if _G.protectionEnabled and humanoid.AutoRotate == false then
                    humanoid.AutoRotate = true
                end
            end))

            table.insert(_G.connections, humanoid:GetPropertyChangedSignal("PlatformStand"):Connect(function()
                if _G.protectionEnabled and humanoid.PlatformStand == true then
                    humanoid.PlatformStand = false
                end
            end))

            table.insert(_G.connections, game:GetService("RunService").RenderStepped:Connect(function()
                if _G.protectionEnabled then
                    if humanoid.Sit and humanoid.SeatPart == nil then
                        humanoid.Sit = false
                    end
                end
            end))
        end

        local function onCharacter(char)
            local humanoid = char:WaitForChild("Humanoid", 5)
            if humanoid and _G.protectionEnabled then
                protectHumanoid(humanoid)
            end
        end

        if _G.protectionEnabled then
            local character = game:GetService("Players").LocalPlayer.Character
            if character then
                onCharacter(character)
            end

            _G.charAddedConn = game:GetService("Players").LocalPlayer.CharacterAdded:Connect(onCharacter)
        else
            clearConnections()
            if _G.charAddedConn then
                _G.charAddedConn:Disconnect()
                _G.charAddedConn = nil
            end
        end
    end
})

AntiTab:AddToggle({
    Name = "アンチ水 (水上歩行)",
    Default = false,
    Callback = function(state)
        local oceanPath = workspace:FindFirstChild("Map")
            and workspace.Map:FindFirstChild("AlwaysHereTweenedObjects")
            and workspace.Map.AlwaysHereTweenedObjects:FindFirstChild("Ocean")
            and workspace.Map.AlwaysHereTweenedObjects.Ocean:FindFirstChild("Object")
            and workspace.Map.AlwaysHereTweenedObjects.Ocean.Object:FindFirstChild("ObjectModel")

        if oceanPath then
            for _, part in pairs(oceanPath:GetChildren()) do
                if part:IsA("BasePart") and part.Name == "Ocean" then
                    part.CanCollide = state
                end
            end
        end
    end
})

AntiTab:AddSection({
	Name = "自動リスポーン"
})

AntiTab:AddDropdown({
    Name = "スポーン場所",
    Default = "Spawn",
    Options = {"Spawn", "Green Safe-House", "Chinese Safe-House", "Blue Safe-House", "Witch Safe-House", "Red Safe-House"},
    Callback = function(Value)
        Config.SelectedSpawnLocation = Value
    end
})

AntiTab:AddToggle({
    Name = "自動リスポーン",
    Default = false,
    Callback = function(Value)
        Config.AutoRespawnTP = Value
        if Config.RespawnConnection then
            Config.RespawnConnection:Disconnect()
            Config.RespawnConnection = nil
        end
        if Value then
            Config.RespawnConnection = localPlayer.CharacterAdded:Connect(function(char)
                local hrp = char:WaitForChild("HumanoidRootPart", 5)
                if hrp then
                    local targetCFrame = SpawnLocations[Config.SelectedSpawnLocation]
                    if targetCFrame then
                        task.wait()
                        hrp.CFrame = targetCFrame
                    end
                end
            end)
        end
    end
})

AntiTab:AddToggle({
    Name = "自動攻撃",
    Default = false,
    Callback = function(enabled)
        Config.airSuspendActive = enabled
        if enabled then
            Config.airSuspendCoroutine = coroutine.create(function()
                while Config.airSuspendActive do
                    task.wait(0.02)

                    local character = localPlayer.Character
                    local head = character and character:FindFirstChild("Head")
                    local localHRP = character and character:FindFirstChild("HumanoidRootPart")
                    if not (character and head and localHRP) then continue end

                    local partOwner = head:FindFirstChild("PartOwner")
                    if not partOwner then continue end

                    local attacker = service.Players:FindFirstChild(partOwner.Value)
                    if not (attacker and attacker.Character) then continue end

                    local targetChar = attacker.Character
                    local hrp = targetChar:FindFirstChild("HumanoidRootPart")
                    local hum = targetChar:FindFirstChildOfClass("Humanoid")
                    local torso = targetChar:FindFirstChild("Torso") or targetChar:FindFirstChild("UpperTorso")

                    if not hrp or not hum then continue end

                    Struggle:FireServer()

                    if Config.SelectedCounterMode == "Death" then
                        pcall(function()
                            SetNetworkOwner:FireServer(hrp, hrp.CFrame)
                            
                            task.wait(0.05)

                            for _, bp in ipairs(targetChar:GetChildren()) do
                                if bp:IsA("BasePart") then
                                    bp.CFrame = CFrame.new(0, -1e9, 0)
                                    bp.CanCollide = false
                                end
                            end

                            hum.Health = 0
                            hum:ChangeState(Enum.HumanoidStateType.Dead)

                            CSV(hrp)

                            if ReplicatedStorage.GrabEvents:FindFirstChild("DestroyGrabLine") then
                                DestroyGrabLine:FireServer(hrp)
                            end
                        end)

                    elseif Config.SelectedCounterMode == "Knockback" then
                        local knockback = Instance.new("BodyVelocity")
                        knockback.MaxForce = Vector3.new(1e6, 1e6, 1e6)
                        knockback.Velocity = (localHRP.CFrame.LookVector * 200) + Vector3.new(0, 50, 0)
                        knockback.Parent = hrp
                        service.Debris:AddItem(knockback, 1)

                    elseif Config.SelectedCounterMode == "airSuspend" and torso then
                        local velocity = torso:FindFirstChild("l") or Instance.new("BodyVelocity")
                        velocity.Name = "l"
                        velocity.Velocity = Vector3.new(0, 5000, 0)
                        velocity.MaxForce = Vector3.new(0, math.huge, 0)
                        velocity.Parent = torso
                        service.Debris:AddItem(velocity, 1)

                    elseif Config.SelectedCounterMode == "Freeze" then
                        local freezeVel = Instance.new("BodyVelocity")
                        freezeVel.MaxForce = Vector3.new(1e6, 1e6, 1e6)
                        freezeVel.Velocity = Vector3.new(0, 0, 0)
                        freezeVel.Parent = hrp
                        service.Debris:AddItem(freezeVel, 2)
                    end
                end
            end)
            coroutine.resume(Config.airSuspendCoroutine)
        else
            if Config.airSuspendCoroutine then
                coroutine.close(Config.airSuspendCoroutine)
                Config.airSuspendCoroutine = nil
            end
        end
    end
})

AntiTab:AddDropdown({
    Name = "モード",
    Options = {"Death", "Knockback", "airSuspend", "Freeze"},
    Default = "Death",
    Callback = function(value)
        Config.SelectedCounterMode = value
    end
})

AntiTab:AddSection({
    Name = "アンチラグ"
})

AntiTab:AddToggle({
    Name = "アンチラグ",
    Default = false,
    Callback = function(state)
        if CharacterAndBeamMove then
            CharacterAndBeamMove.Disabled = state
        end
    end
})

AntiTab:AddSection({
	Name = "アンチキック"
})

AntiTab:AddToggle({
    Name = "アンチキックグラブ",
    Default = false,
    Callback = function(enabled)
        if enabled then
            local character = localPlayer.Character

            Config.antiKickCoroutine = service.RunService.Heartbeat:Connect(function()
                local character = localPlayer.Character
                if character and character:FindFirstChild("HumanoidRootPart") and character:FindFirstChild("HumanoidRootPart"):FindFirstChild("FirePlayerPart") then
                    local partOwner = character:FindFirstChild("HumanoidRootPart"):FindFirstChild("FirePlayerPart"):FindFirstChild("PartOwner")
                    if partOwner and partOwner.Value ~= localPlayer.Name then
                        local args = {[1] = character:WaitForChild("HumanoidRootPart"), [2] = 0}
                        RagdollRemote:FireServer(unpack(args))
                        wait(0.1)
                        Struggle:FireServer()
                    end
                end
            end)
        else
            if Config.antiKickCoroutine then
                Config.antiKickCoroutine:Disconnect()
                Config.antiKickCoroutine = nil
            end
        end
    end
})

AntiTab:AddToggle({
    Name = "アンチキック",
    Default = false,
    Callback = function(Value)
        local ToyContents = PlayerGui:WaitForChild("MenuGui"):WaitForChild("Menu"):WaitForChild("TabContents"):WaitForChild("Toys"):WaitForChild("Contents")
        
        Config.antikickV2 = Value

        if Value then
            Config.notifiedNoCoins = false
        end

        local function getCharPart(name)
            local char = localPlayer.Character
            return char and char:FindFirstChild(name)
        end

        local function inner_toyspn(toyName, cf)
            SpawnToyRemoteFunction:InvokeServer(toyName, cf, Vector3.new(0, 0, 0))
        end

        local function inner_toyspn2(toyName)
            local hrp = getCharPart("HumanoidRootPart")
            if hrp then
                local cf = hrp.CFrame
                inner_toyspn(toyName, cf - Vector3.new(cf.LookVector.X * 20, -15, cf.LookVector.Z * 20))
            end
        end

        local function f()
            local character = localPlayer.Character
            if not character then return end
            
            local hrp = character:FindFirstChild("HumanoidRootPart")
            local hum = character:FindFirstChildOfClass("Humanoid")
            local leg = character:FindFirstChild("Right Leg")

            if not hrp or not hum or not leg or hum.Health <= 0 then return end
            if localPlayer.InPlot.Value then return end

            if not ToyContents:FindFirstChild("NinjaKunai") then
                local success = BuyToyRemoteFunction:InvokeServer("NinjaKunai")
                
                if not success then
                    if not Config.notifiedNoCoins then
                        NBNotification("コインが足りないためNinjaKunaiを購入できません。")
                        Config.notifiedNoCoins = true
                    end
                    return
                end
            end

            local kunai = SpawnedInToys:FindFirstChild("NinjaKunai")
            
            if kunai then
                local sticky = kunai:WaitForChild("StickyPart", 1)
                if sticky then
                    local weld = sticky:FindFirstChild("StickyWeld")
                    local attachedPart = weld and weld.Part1

                    if not weld or attachedPart ~= leg then
                        DestroyToy:FireServer(kunai)
                        task.wait(0.1)
                    end
                else
                    DestroyToy:FireServer(kunai)
                    task.wait(0.1)
                end
            elseif localPlayer.CanSpawnToy.Value then
                task.spawn(inner_toyspn2, "NinjaKunai")
                
                local newKunai = SpawnedInToys:WaitForChild("NinjaKunai", 3)
                if newKunai then
                    local sticky = newKunai:WaitForChild("StickyPart", 2)
                    if sticky then
                        local weld = sticky:WaitForChild("StickyWeld", 2)

                        SetNetworkOwner:FireServer(sticky, sticky.CFrame)

                        local retry = 0
                        while Config.antikickV2 and weld.Part1 == nil and retry < 20 do
                            StickyPartEvent:FireServer(sticky, leg,
                                CFrame.new(0.0490287527, 0.5, 0, 0, 0.00739139877, -0.999561906,
                                    -0.998452604, -0.0478846952, 0.0282763243, -0.0476547107, 0.99882561,
                                    0) * CFrame.Angles(0, 0, 0))
                            task.wait(0.1)
                            retry = retry + 1
                        end
                    end
                end
            end
        end

        if Config.antikickV2 then
            task.spawn(function()
                while Config.antikickV2 do
                    local success, err = pcall(f)
                    task.wait()
                end
            end)
        end
    end
})

PlayerTab:AddToggle({
    Name = "歩行速度",
    Default = false,
    Callback = function(Value)
        Config.walkSpeedToggle = Value
        task.spawn(function()
            while Config.walkSpeedToggle do
                local char = localPlayer.Character
                if char and char:FindFirstChild("Humanoid") and char:FindFirstChild("HumanoidRootPart") then
                    local hrp = char.HumanoidRootPart
                    local moveDir = char.Humanoid.MoveDirection
                    hrp.CFrame = hrp.CFrame + moveDir * (Config.walkSpeedValue / 10)
                end
                task.wait()
            end

            local char = localPlayer.Character
            if char and char:FindFirstChild("Humanoid") then
                char.Humanoid.WalkSpeed = 16
            end
        end)
    end
})

PlayerTab:AddSlider({
    Name = "速度",
    Min = 0,
    Max = 300,
    Default = Config.walkSpeedValue,
    Increment = 1,
    Callback = function(Value)
        Config.walkSpeedValue = Value
        local char = localPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid.WalkSpeed = Value
        end
    end
})

PlayerTab:AddToggle({
    Name = "無限ジャンプ",
    Default = false,
    Callback = function(Value)
        Config.infiniteJumpToggle = Value

        if Config.infiniteJumpToggle then
            if Config.jumpConnection then Config.jumpConnection:Disconnect() end
            Config.jumpConnection = service.UserInputService.JumpRequest:Connect(function()
                local char = localPlayer.Character
                local humanoid = char and char:FindFirstChildOfClass("Humanoid")
                if humanoid then
                    humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
                end
            end)
        else
            if Config.jumpConnection then
                Config.jumpConnection:Disconnect()
                Config.jumpConnection = nil
            end
        end
    end
})

PlayerTab:AddSlider({
    Name = "ジャンプ力",
    Min = 24,
    Max = 300,
    Default = Config.jumpPowerValue,
    Increment = 1,
    Callback = function(Value)
        Config.jumpPowerValue = Value
        local char = localPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            local hum = char.Humanoid
            hum.UseJumpPower = true
            hum.JumpPower = Value
        end
    end
})

PlayerTab:AddToggle({
    Name = "重力",
    Default = false,
    Callback = function(Value)
        Config.GravityToggle = Value

        if Value then
            workspace.Gravity = Config.gravityPower
        else
            workspace.Gravity = Config.defGravity
        end
    end
})

PlayerTab:AddSlider({
    Name = "重力",
    Min = 0,
    Max = 1000,
    Default = Config.gravityPower,
    Increment = 10,
    Callback = function(Value)
        Config.gravityPower = Value

        if Config.GravityToggle then
            workspace.Gravity = Value
        end
    end
})

PlayerTab:AddSection({
	Name = "カメラ"
})

PlayerTab:AddToggle({
    Name = "三人称視点",
    Default = false,
    Callback = function(Value)

        local humanoid = Character and Character:FindFirstChild("Humanoid")
        if not humanoid then return end

        Camera.CameraType = Enum.CameraType.Custom
        Camera.CameraSubject = humanoid

        if Value then
            localPlayer.CameraMode = Enum.CameraMode.Classic
            localPlayer.CameraMaxZoomDistance = 16456456546
            localPlayer.CameraMinZoomDistance = 0.5
        else
            localPlayer.CameraMode = Enum.CameraMode.LockFirstPerson
            localPlayer.CameraMaxZoomDistance = 0
            localPlayer.CameraMinZoomDistance = 0
        end

    end
})

PlayerTab:AddToggle({
    Name = "視野角",
    Default = false,
    Callback = function(Value)
        Config.fovToggle = Value
        if Config.fovToggle then
            Camera.FieldOfView = Config.fov
        else
            Camera.FieldOfView = Config.deffov
        end
    end
})

PlayerTab:AddSlider({
    Name = "視野角",
    Min = 10,
    Max = 120,
    Default = Config.fov,
    Increment = 1,
    Callback = function(Value)
        Config.fov = Value
        if Config.fovToggle then
            Camera.FieldOfView = Config.fov
        end
    end
})

PlayerTab:AddSection({
	Name = "ノークリップ"
})

PlayerTab:AddToggle({
    Name = "ノークリップ",
    Default = false,
    Callback = function(Value)
        Config.noclip = Value
        if Config.NoclipConnection then
            Config.NoclipConnection:Disconnect()
            Config.NoclipConnection = nil
        end

        if Config.noclip then
            Config.NoclipConnection = service.RunService.Stepped:Connect(function()
                local character = GetCharacter()
                if character then
                    for _, part in ipairs(character:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.CanCollide = false
                        end
                    end
                end
            end)
        end
    end    
})

PlayerTab:AddSection({
	Name = "その他"
})

PlayerTab:AddSlider({
    Name = "FPS制限",
    Min = 2,
    Max = 2000,
    Default = 300,
    Increment = 1,
    Callback = function(fpsCap1)
        setfpscap(fpsCap1)
    end
})

PlayerTab:AddToggle({
    Name = "フリーズ",
    Default = false,
    Callback = function(state)
        local char = localPlayer.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        if not root then return end

        if state then
            Config.freeze = root.CFrame
            root.Anchored = true
        else
            root.Anchored = false
            if Config.freeze then
                root.CFrame = Config.freeze
            end
        end
    end
})

PlayerTab:AddSection({Name = "TPWalk"})
PlayerTab:AddToggle({
    Name = "TPWalk",
    Default = false,
    Callback = function(v)
        UtilityConfig.TPWalk = v
        if v then enableTPWalk2() else disableTPWalk2() end
    end
})
PlayerTab:AddSlider({
    Name = "TPWalk速度",
    Min = 16, Max = 500, Default = 50, Increment = 5,
    Callback = function(v)
        UtilityConfig.TPWalkSpeed = v
        if UtilityConfig.TPWalk then disableTPWalk2(); enableTPWalk2() end
    end
})

LineTab:AddSection({
	Name = "インフライン"
})

LineTab:AddToggle({
    Name = "インフライン",
    Default = false,
    Callback = function(state)
        Config.InfLineToggle = state
    end,
})

LineTab:AddSlider({
    Name = "ライン距離",
    Min = Config.MinExtendLine,
    Max = Config.MaxExtendLine,
    Default = 3,
    Increment = 1,
    Callback = function(value)
        Config.LineDistance = value
    end,
})

LineTab:AddSection({
	Name = "ライン"
})

LineTab:AddToggle({
    Name = "レインボーライン",
    Default = false,
    Callback = function(Value)
        Config.ToggleState = Value

        if Value then
            if not Config.originalColors then
                if service.ReplicatedStorage:FindFirstChild("DataEvents") and service.ReplicatedStorage.DataEvents:FindFirstChild("UpdateLineColorsEvent") then
                    Config.originalColors = {}
                    for i = 1, 10 do
                        table.insert(Config.originalColors, Color3.new(1, 1, 1))
                    end
                end
            end

            task.spawn(function()
                Config.hueOffset = 0
                while Config.ToggleState do
                    if service.ReplicatedStorage:FindFirstChild("DataEvents") and service.ReplicatedStorage.DataEvents:FindFirstChild("UpdateLineColorsEvent") then
                        Config.hueOffset = (Config.hueOffset + 0.01) % 1
                        
                        local cs = ColorSequence.new{
                            ColorSequenceKeypoint.new(0,   Color3.fromHSV((0   + Config.hueOffset)%1,1,1)),
                            ColorSequenceKeypoint.new(0.1, Color3.fromHSV((0.1 + Config.hueOffset)%1,1,1)),
                            ColorSequenceKeypoint.new(0.2, Color3.fromHSV((0.2 + Config.hueOffset)%1,1,1)),
                            ColorSequenceKeypoint.new(0.3, Color3.fromHSV((0.3 + Config.hueOffset)%1,1,1)),
                            ColorSequenceKeypoint.new(0.4, Color3.fromHSV((0.4 + Config.hueOffset)%1,1,1)),
                            ColorSequenceKeypoint.new(0.5, Color3.fromHSV((0.5 + Config.hueOffset)%1,1,1)),
                            ColorSequenceKeypoint.new(0.6, Color3.fromHSV((0.6 + Config.hueOffset)%1,1,1)),
                            ColorSequenceKeypoint.new(0.7, Color3.fromHSV((0.7 + Config.hueOffset)%1,1,1)),
                            ColorSequenceKeypoint.new(0.8, Color3.fromHSV((0.8 + Config.hueOffset)%1,1,1)),
                            ColorSequenceKeypoint.new(0.9, Color3.fromHSV((0.9 + Config.hueOffset)%1,1,1)),
                            ColorSequenceKeypoint.new(1,   Color3.fromHSV((1   + Config.hueOffset)%1,1,1))
                        }

                        service.ReplicatedStorage.DataEvents.UpdateLineColorsEvent:FireServer(
                            cs,
                            cs.Keypoints[1].Value,
                            cs.Keypoints[2].Value,
                            cs.Keypoints[3].Value,
                            cs.Keypoints[4].Value,
                            cs.Keypoints[5].Value,
                            cs.Keypoints[6].Value,
                            cs.Keypoints[7].Value,
                            cs.Keypoints[8].Value,
                            cs.Keypoints[9].Value
                        )
                    end
                    task.wait(0.05)
                end
            end)
        else
            if Config.originalColors then
                pcall(function()
                    service.ReplicatedStorage.DataEvents.UpdateLineColorsEvent:FireServer(
                        Config.originalColors[1],
                        Config.originalColors[2],
                        Config.originalColors[3],
                        Config.originalColors[4],
                        Config.originalColors[5],
                        Config.originalColors[6],
                        Config.originalColors[7],
                        Config.originalColors[8],
                        Config.originalColors[9],
                        Config.originalColors[10]
                    )
                end)
            end
        end
    end
})

LineTab:AddSection({
	Name = "ラインで選択したプレイヤーをラグらせる"
})

PlayerDropdown = LineTab:AddDropdown({
    Name = "ラインのプレイヤーを選択",
    Options = Config.PlayerNames,
    MultiSelect = true,
    Callback = function(selectedDisplay)
        Config.SelectedPlayer = Config.NameMap[selectedDisplay]
    end
})

service.Players.PlayerAdded:Connect(RefreshPlayer)
service.Players.PlayerRemoving:Connect(RefreshPlayer)

LineTab:AddToggle({
    Name = "クレイジーライン",
    Default = false,
    Callback = function(state)
        Config.IsRagServer = state
        if Config.IsRagServer then
            coroutine.wrap(function()
                while Config.IsRagServer do
                    if Config.SelectedPlayer then
                        local targetPlayer = game.Players:FindFirstChild(Config.SelectedPlayer)
                        local playerCharacter = targetPlayer and targetPlayer.Character
                        
                        if playerCharacter then
                            for i = 1, Config.LineAmount do
                                local targetPart = playerCharacter:FindFirstChildOfClass("Part")
                                if targetPart then
                                    CreateGrabLine:FireServer(
                                        targetPart,
                                        CFrame.new(0, 0, 0)
                                    )
                                end
                            end
                        end
                    end
                    task.wait()
                end
            end)()
        end
    end
})

LineTab:AddSection({
	Name = "ライン"
})
local CrazyLineLag = false
LineTab:AddToggle({
    Name = "クレイジーライン",
    Default = false,
    Callback = function(state)
        CrazyLineLag = state
        
        if state then
            task.spawn(function()
                while CrazyLineLag do
                    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
                        if not CrazyLineLag then break end
                        
                        if player ~= localPlayer and player.Character then
                            local torso = player.Character:FindFirstChild("Torso") or player.Character:FindFirstChild("UpperTorso")
                            if torso then
                                CreateGrabLine:FireServer(
                                    torso,
                                    CFrame.new(
                                        0.12640380859375,
                                        0.9606337547302246,
                                        -0.5000009536743164,
                                        0.9985212683677673,
                                        0,
                                        -0.05436277016997337,
                                        -6.4805472099749295e-9,
                                        1,
                                        -1.1903301100346653e-7,
                                        0.05436277016997337,
                                        5.960464477539063e-8,
                                        0.9985212683677673
                                    )
                                )
                            end
                        end
                    end
                    task.wait()
                end
            end)
        end
    end
})

LineTab:AddToggle({
    Name = "見えないライン",
    Default = false,
    Callback = function(Value)
        invisline = Value
        while invisline do
            CreateGrabLine:FireServer()
            task.wait()
        end
    end
})

LineTab:AddSection({
	Name = "サーバーラグ"
})

LineTab:AddToggle({
    Name = "サーバーをラグらせる",
    Default = false,
    Callback = function(state)
        Config.Lagserver = state

        while Config.Lagserver do
            for i = 1, Config.LagS do
                for _, player in ipairs(service.Players:GetPlayers()) do
                    local character = player.Character
                    local torso = character and character:FindFirstChild("Torso")

                    if torso then
                        CreateGrabLine:FireServer(torso, torso.CFrame)
                    end
                end
            end

            task.wait(1)
        end
    end
})

LineTab:AddSlider({
    Name = "ラグ",
    Min = 1,
    Max = 2000,
    Default = 150,
    Increment = 1,
    Callback = function(value)
        Config.LagS = value
    end
})

TeleportTab:AddDropdown({
    Name = "テレポート先",
    Default = "Green House",
    Options = GetTableKeys(placeLocations),
    Callback = function(selected)
        PlaceTP = selected
    end
})

TeleportTab:AddButton({
    Name = "テレポート",
    Callback = function()
        TeleportPlayer(placeLocations[PlaceTP])
    end
})

local PlayerDropdown
PlayerDropdown = TeleportTab:AddDropdown({
    Name = "プレイヤーを選択",
    Default = "",
    Options = GetPlayerList(),
    Callback = function(selected)
        Config.PlayerToTeleport = Config.PlayerMap[selected]
    end
})

TeleportTab:AddButton({
    Name = "テレポート",
    Callback = function()
        local targetPlayer = service.Players:FindFirstChild(Config.PlayerToTeleport)
        local myRoot = GetPlayerRoot()

        if targetPlayer and targetPlayer.Character and myRoot then
            local hrp = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                TeleportWithOffset(
                    hrp.CFrame,
                    myRoot,
                    targetPlayer.Character,
                    targetPlayer.Name
                )
            end
        end
    end
})

LoopTeleportToggle = TeleportTab:AddToggle({
    Name = "ループテレポート",
    Default = false,
    Callback = function(state)
        Config.LoopPlayerTP = state

        if state then
            task.spawn(function()
                while Config.LoopPlayerTP do
                    local player = service.Players:FindFirstChild(Config.PlayerToTeleport)
                    if not player then
                        Config.LoopTeleportToggle:Set(false)
                        break
                    end

                    local char = player.Character
                    local hrp = char and char:FindFirstChild("HumanoidRootPart")
                    local myRoot = GetPlayerRoot()

                    if hrp and myRoot then
                        TeleportWithOffset(
                            hrp.CFrame,
                            myRoot,
                            char,
                            player.Name
                        )
                    end

                    task.wait()
                end
            end)
        end
    end
})

TeleportTab:AddToggle({
    Name = "カメラ固定",
    Default = false,
    Callback = function(state)
        Config.LockCameraOnPlayer = state
        if Config.CameraConnection then Config.CameraConnection:Disconnect() end
        if state then
            Config.CameraConnection = service.RunService.RenderStepped:Connect(function()
                local player = service.Players:FindFirstChild(Config.PlayerToTeleport)
                local camera = service.Workspace.CurrentCamera
                if not Config.LockCameraOnPlayer then
                    Config.CameraConnection:Disconnect()
                    return
                end
                if player and player.Character then
                    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        camera.CFrame = CFrame.lookAt(camera.CFrame.Position, hrp.Position + Vector3.new(0,1,0))
                    end
                end
            end)
        end
    end
})

TeleportTab:AddToggle({
    Name = "ビュー",
    Default = false,
    Callback = function(state)
        Config.ViewCameraOnPlayer = state
        local originalSubject = Camera.CameraSubject
        while Config.ViewCameraOnPlayer do
            local player = service.Players:FindFirstChild(Config.PlayerToTeleport)
            if player and player.Character then
                local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
                if humanoid then
                    Camera.CameraSubject = humanoid
                end
            end
            task.wait()
        end
        Camera.CameraSubject = originalSubject
    end
})

TeleportTab:AddSlider({
    Name = "オフセット",
    Min = 1,
    Max = 20,
    Default = 1,
    Increment = 1,
    Callback = function(value)
        Config.TeleportPlayerOffset = value
    end,
})

TeleportTab:AddDropdown({
    Name = "方向",
    Default = "Behind",
    Options = {"Behind", "Left", "Right", "Front"},
    Callback = function(selected)
        Config.PlayerToTeleportDirection = selected
    end
})

NotifyTab:AddToggle({
	Name = "参加通知",
	Default = false,
	Callback = function(enabled)
		Config.joinNotify = enabled
	end
})

NotifyTab:AddToggle({
	Name = "退出通知",
	Default = false,
	Callback = function(enabled)
		Config.leaveNotify = enabled
	end
})

NotifyTab:AddToggle({
    Name = "キック通知",
    Default = false,
    Callback = function(Value)
        if Config.kickConnection then
            Config.kickConnection:Disconnect()
            Config.kickConnection = nil
        end

        if Value then
            Config.kickConnection = workspace.ChildAdded:Connect(function(part)
                if part.Name ~= "BlackHoleKick" then return end
                
                part.Name = "BlackHoleDetected"
                
                local kicklist = {}
                local kicklistDis = {}
                for _, player in ipairs(service.Players:GetPlayers()) do
                    table.insert(kicklist, player.Name)
                    table.insert(kicklistDis, player.DisplayName)
                end

                task.wait()

                for i, name in ipairs(kicklist) do
                    if not service.Players:FindFirstChild(name) then
                        local message = string.format("%s (%s) がゲームからキックされました。", kicklistDis[i], name)
                        NBNotification(message)
                        return
                    end
                end
            end)
        end
    end    
})

NotifyTab:AddToggle({
    Name = "ブロブマンスポーン通知",
    Default = false,
    Callback = function(state)
        Config.blobWebhookToggle = state

        if Config.BlobConnections then
            for _, conn in pairs(Config.BlobConnections) do conn:Disconnect() end
            table.clear(Config.BlobConnections)
        else
            Config.BlobConnections = {}
        end

        if Config.BlobPlayerAddedConnection then
            Config.BlobPlayerAddedConnection:Disconnect()
            Config.BlobPlayerAddedConnection = nil
        end

        if state then
            local function watch(folder, plr)
                if not folder then return end
                local conn = folder.ChildAdded:Connect(function(child)
                    if child.Name == "CreatureBlobman" and plr ~= localPlayer then
                        NBNotification(string.format("%s (%s) がブロブマンを出しました。", plr.DisplayName, plr.Name))
                    end
                end)
                table.insert(Config.BlobConnections, conn)
            end

            for _, plr in pairs(service.Players:GetPlayers()) do
                watch(service.Workspace:FindFirstChild(plr.Name .. "SpawnedInToys"), plr)
            end

            Config.BlobPlayerAddedConnection = service.Players.PlayerAdded:Connect(function(plr)
                task.wait(1)
                watch(service.Workspace:FindFirstChild(plr.Name .. "SpawnedInToys"), plr)
            end)
        end
    end
})

AuraTab:AddToggle({
    Name = "グラブオーラ",
    Default = false,
    Callback = function(enabled)
        local function getPlayersInRange(center, radius)
            local playersInRange = {}
            for _, player in pairs(service.Players:GetPlayers()) do
                if player ~= game:GetService("Players").LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    local playerTorso = player.Character:FindFirstChild("Torso") or player.Character:FindFirstChild("HumanoidRootPart")
                    if playerTorso then
                        local distance = (playerTorso.Position - center.Position).Magnitude
                        if distance <= radius then
                            table.insert(playersInRange, player)
                        end
                    end
                end
            end
            return playersInRange
        end

        if enabled then
            Config.auraCoroutine = coroutine.create(function()
                while true do
                    local lp = game:GetService("Players").LocalPlayer
                    if lp.Character and lp.Character:FindFirstChild("HumanoidRootPart") then
                        local humanoidRootPart = lp.Character.HumanoidRootPart
                        local targets = getPlayersInRange(humanoidRootPart, Config.auraRadius)
                        
                        for _, player in pairs(targets) do
                            coroutine.wrap(function()
                                local playerCharacter = player.Character
                                local playerTorso = playerCharacter:FindFirstChild("Torso") or playerCharacter:FindFirstChild("HumanoidRootPart")
                                if playerTorso then
                                    SetNetworkOwner:FireServer(playerTorso, playerCharacter.HumanoidRootPart.CFrame)

                                    local velocity = Instance.new("BodyVelocity")
                                    velocity.Parent = playerTorso
                                    velocity.Velocity = Vector3.zero
                                    velocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
                                    game:GetService("Debris"):AddItem(velocity, 0.1)
                                end
                            end)()
                        end
                    end
                    wait(0.1)
                end
            end)
            coroutine.resume(Config.auraCoroutine)
        else
            if Config.auraCoroutine then
                coroutine.close(Config.auraCoroutine)
                Config.auraCoroutine = nil
            end
        end
    end
})

AuraTab:AddToggle({
	Name = "着席オーラ",
	Default = false,
	Callback = function(Value)

		Config.SitAuraToggle = Value

		if Value then
			onSitaura()
		else
			offSitAura()
		end

	end
})

AuraTab:AddToggle({
	Name = "スピンオーラ",
	Default = false,
	Callback = function(enabled)
		if enabled then
			onSpinAura()
		else
			offSpinAura()
		end
	end
})

AuraTab:AddToggle({
    Name = "打ち上げオーラ",
    Default = false,
    Callback = function(state)
        if state then
            onAirSuspendAura()
        else
            offAirSuspendAura()
        end
    end
})

AuraTab:AddToggle({
    Name = "ラグドールオーラ",
    Default = false,
    Callback = function(v)
        Config.RagdollAuraToggle = v
    end
})

AuraTab:AddToggle({
	Name = "テレキネシスオーラ",
	Default = false,
	Callback = function(enabled)
		if enabled then
			onHellSendAura()
		else
			offHellSendAura()
		end
	end
})

AuraTab:AddToggle({
    Name = "デスオーラ",
    Default = false,
    Callback = function(state)
        DeathAura(state)
    end
})
AuraTab:AddToggle({
    Name = "引き寄せオーラ",
    Default = false,
    Callback = function(enabled)
        _G.AttractionAura = enabled

        if enabled then
            AttractionAura()
        end
    end
})

AuraTab:AddToggle({
    Name = "ボイドオーラ",
    Default = false,
    Callback = function(v)
        Config.VoidAura = v

        if v then
            local function setVelocity(part, velocityVector)
                if not part or not part:IsA("BasePart") then return end
                
                part.AssemblyLinearVelocity = velocityVector
                
                local bv = Instance.new("BodyVelocity")
                bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
                bv.Velocity = velocityVector
                bv.Parent = part
                
                service.Debris:AddItem(bv, 0.1)
            end

            task.spawn(function()
                while Config.VoidAura do
                    local myChar = GetCharacter()
                    local myRoot = myChar and myChar:FindFirstChild("HumanoidRootPart")
                    
                    if myRoot then
                        for _, player in ipairs(service.Players:GetPlayers()) do
                            if CanAuraTarget(player) then
                                local targetChar = player.Character
                                local targetRoot = targetChar and targetChar:FindFirstChild("HumanoidRootPart")

                                if targetRoot and (targetRoot.Position - myRoot.Position).Magnitude <= Config.auraRadius then
                                    ROS(targetRoot, targetRoot.CFrame)
                                    setVelocity(targetRoot, Vector3.new(0, 10000, 0))
                                end
                            end
                        end
                    end
                    
                    task.wait(0.5)
                end
            end)
        end
    end
})

AuraTab:AddToggle({
    Name = "ノークリップオーラ",
    Default = false,
    Callback = function(v)
        Config.NoclipAura = v
    end
})

AuraTab:AddSection({
	Name = "フリングオーラ"
})

AuraTab:AddToggle({
    Name = "フリングオーラ",
    Default = false,
    Callback = function(state)
        Config.FlingAura = state
        if state then
            StartFling()
        end
    end
})

AuraTab:AddDropdown({
    Name = "タイプ",
    Default = "プレイヤー",
    Options = {
        "プレイヤー",
        "オブジェクト",
        "プレイヤー & オブジェクト"
    },
    Callback = function(Option)
        if Option == "プレイヤー" then
            Config.FlingTarget = 1
        elseif Option == "オブジェクト" then
            Config.FlingTarget = 2
        else
            Config.FlingTarget = 3
        end
    end
})

AuraTab:AddSlider({
    Name = "フリングパワー",
    Min = 200,
    Max = 3000,
    Default = 200,
    Increment = 50,
    Callback = function(value)
        Config.FlingPower = value
    end
})

AuraTab:AddSection({
	Name = "ホワイトリスト"
})

AuraTab:AddToggle({
    Name = "フレンドをホワイトリストに登録",
    Default = false,
    Callback = function(state)
        Config.WhitelistFriends = state
    end
})

bindTab:AddToggle({
    Name = "全バインドを有効化",
    Default = false,
    Callback = function(Value)
        Config.BindEnabled = Value
    end
})

bindTab:AddBind({
    Name = "クリックTP",
    Default = Enum.KeyCode.Z,
    Hold = false,
    Callback = function()
        if not Config.BindEnabled then return end

        local character = _G.ControllingCreature or localPlayer.Character
        if not character then return end

        local originPartName = _G.ControllingCreature and "Head" or (character:FindFirstChild("CamPart") and "CamPart")
        if not originPartName or not character:FindFirstChild(originPartName) then return end

        local originPart = character[originPartName]
        local camPart = character:FindFirstChild("CamPart")
        if not camPart then return end
        
        local lookVector = camPart.CFrame.lookVector
        local tpRay = Ray.new(originPart.Position, lookVector * 5000)
        local hitPart, hitPosition = game:GetService("Workspace"):FindPartOnRayWithIgnoreList(tpRay, {character})

        if hitPart then
            character.HumanoidRootPart.CFrame = CFrame.new(hitPosition.X, hitPosition.Y + 5, hitPosition.Z)
        end
    end    
})

bindTab:AddBind({
    Name = "アンカー",
    Default = Enum.KeyCode.K,
    Hold = false,
    Callback = function()
        if not Config.BindEnabled then return end

        local grabParts = workspace:FindFirstChild("GrabParts")
        if not grabParts then return end

        local grabPart = grabParts:FindFirstChild("GrabPart")
        if not grabPart then return end

        local weld = grabPart:FindFirstChild("WeldConstraint")
        if not weld or not weld.Part1 then return end

        local targetModel = weld.Part1:FindFirstAncestorOfClass("Model") or weld.Part1
        if not targetModel then return end

        local part = targetModel.PrimaryPart or weld.Part1
        local bp = part:FindFirstChildOfClass("BodyPosition")
        local bg = part:FindFirstChildOfClass("BodyGyro")

        if bp or bg then
            for _, v in ipairs(part:GetChildren()) do
                if v:IsA("BodyPosition") or v:IsA("BodyGyro") then v:Destroy() end
            end
            local h = targetModel:FindFirstChild("Highlight")
            if h then h:Destroy() end
            return
        end

        local newBP = Instance.new("BodyPosition")
        newBP.Position = part.Position
        newBP.MaxForce = Vector3.new(1e6, 1e6, 1e6)
        newBP.P = 5000
        newBP.D = 500
        newBP.Parent = part

        local newBG = Instance.new("BodyGyro")
        newBG.CFrame = part.CFrame
        newBG.MaxTorque = Vector3.new(1e6, 1e6, 1e6)
        newBG.P = 5000
        newBG.D = 500
        newBG.Parent = part

        local highlight = Instance.new("Highlight")
        highlight.Name = "Highlight"
        highlight.FillTransparency = 1
        highlight.OutlineColor = Color3.fromRGB(0, 0, 255)
        highlight.Parent = targetModel

        task.spawn(function()
            while highlight.Parent do
                game:GetService("TweenService"):Create(highlight, TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {OutlineTransparency = 0.4}):Play()
                task.wait(1)
                game:GetService("TweenService"):Create(highlight, TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {OutlineTransparency = 0}):Play()
                task.wait(1)
            end
        end)

        local tempSound = Instance.new("Sound")
        tempSound.SoundId = "rbxassetid://18595195017"
        tempSound.Parent = game:GetService("SoundService")
        tempSound:Play()
        tempSound.Ended:Connect(function() tempSound:Destroy() end)
    end
})

bindTab:AddBind({
    Name = "ズーム",
    Default = Enum.KeyCode.C,
    Hold = false,
    Callback = function()
        if not Config.BindEnabled then return end

        Config.ZoomBind = not Config.ZoomBind
        local Camera = workspace.CurrentCamera

        if Config.ZoomBind then
            Config.DefaultFOV = Camera.FieldOfView
            Camera.FieldOfView = Config.ZFOV or 30
        else
            Camera.FieldOfView = Config.DefaultFOV or 70
        end
    end
})

ObjectTab:AddSection({
    Name = "ハート"
})

ObjectTab:AddToggle({
    Name = "ハート",
    Default = false,
    Callback = function(v)
        Config.HeartToggle = v
        
        if Config.HeartLoopConn then 
            Config.HeartLoopConn:Disconnect() 
            Config.HeartLoopConn = nil 
        end
        
        if Config.HeartPoints then
            for _, p in ipairs(Config.HeartPoints) do
                if p.data then
                    if p.data.part then
                        p.data.part.CanCollide = true
                        for _, child in ipairs(p.data.part.Parent:GetChildren()) do
                            if child:IsA("BasePart") then child.CanCollide = true end
                        end
                    end
                    if p.data.BG then p.data.BG:Destroy() end
                    if p.data.BP then p.data.BP:Destroy() end
                end
            end
        end
        Config.HeartPoints = {}

        if v then
            local toys = {}
            for _, obj in ipairs(workspace:GetDescendants()) do
                if obj:IsA("Model") and obj.Name == "FireworkSparkler" then table.insert(toys, obj) end
            end
            local count = math.min(#toys, Config.ObjectCount)
            for i = 1, count do
                local toy = toys[i]
                local part = toy.PrimaryPart or (function() for _, c in ipairs(toy:GetChildren()) do if c:IsA("BasePart") then return c end end end)()
                if part then
                    for _, c in ipairs(toy:GetChildren()) do 
                        if c:IsA("BasePart") then 
                            c.CanCollide = false 
                            c.Anchored = false 
                        end 
                    end
                    
                    local bp = Instance.new("BodyPosition") 
                    bp.MaxForce = Vector3.new(1,1,1)*1e10 
                    bp.P, bp.D = 15000, 200 
                    bp.Parent = part
                    
                    local bg = Instance.new("BodyGyro") 
                    bg.MaxTorque = Vector3.new(1,1,1)*1e10 
                    bg.P, bg.D = 15000, 200 
                    bg.Parent = part
                    
                    table.insert(Config.HeartPoints, {
                        angle = (i-1)*(2*math.pi/count), 
                        data = {part = part, BG = bg, BP = bp}
                    })
                end
            end
            
            Config.HeartTime = 0
            Config.HeartLoopConn = service.RunService.RenderStepped:Connect(function(dt)
                local baseCFrame = CFrame.new()

                if Config.FollowMode == "Follow Player" then
                    local plr = Config.TargetPlayer or localPlayer
                    if plr and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                        baseCFrame = plr.Character.HumanoidRootPart.CFrame
                        Config.FrozenCFrame = baseCFrame
                    end
                else
                    baseCFrame = Config.FrozenCFrame or CFrame.new(0, 10, 0)
                end

                Config.HeartTime = Config.HeartTime + dt
                local pulse = math.sin(Config.HeartTime * Config.PulseSpeed) * Config.PulseAmplitude
                
                for _, point in ipairs(Config.HeartPoints) do
                    local d = point.data
                    local angle = point.angle + Config.HeartTime * Config.RotationSpeed
                    local scale = Config.Size / 20
                    local x = 16 * (math.sin(angle)^3) * scale
                    local y = (13*math.cos(angle) - 5*math.cos(2*angle) - 2*math.cos(3*angle) - math.cos(4*angle)) * scale
                    
                    if pulse ~= 0 then 
                        local f = 1 + pulse * 0.1 
                        x, y = x * f, y * f 
                    end
                    
                    local offsetPos
                    local finalRotation

                    if Config.OrientationMode == "Vertical" then
                        offsetPos = baseCFrame.RightVector * x + baseCFrame.UpVector * (y + Config.Height)
                        finalRotation = baseCFrame.Rotation
                    elseif Config.OrientationMode == "Right" then
                        offsetPos = baseCFrame.LookVector * x + baseCFrame.UpVector * (y + Config.Height)
                        finalRotation = baseCFrame.Rotation * CFrame.Angles(0, math.rad(90), 0)
                    elseif Config.OrientationMode == "Left" then
                        offsetPos = baseCFrame.LookVector * -x + baseCFrame.UpVector * (y + Config.Height)
                        finalRotation = baseCFrame.Rotation * CFrame.Angles(0, math.rad(-90), 0)
                    else
                        offsetPos = baseCFrame.RightVector * x + baseCFrame.LookVector * y + Vector3.new(0, Config.Height, 0)
                        finalRotation = CFrame.Angles(0, math.rad(180), 0)
                    end

                    d.BP.Position = baseCFrame.Position + offsetPos
                    d.BG.CFrame = CFrame.new(d.BP.Position) * finalRotation
                end
            end)
        end
    end
})

ObjectTab:AddDropdown({
    Name = "フォローモード",
    Default = "Follow Player",
    Options = {"Follow Player", "Freeze"},
    Callback = function(v) Config.FollowMode = v end
})

local PlayerDropdown = ObjectTab:AddDropdown({
    Name = "プレイヤーを選択",
    Default = localPlayer.DisplayName .. " (ID: " .. localPlayer.Name .. ")",
    Options = {},
    Callback = function(selected)
        local name = selected:match("%(ID: (.-)%)")
        if name then
            Config.TargetPlayer = service.Players:FindFirstChild(name)
        end
    end
})

local function UpdatePlayerList2()
    local list = {}

    for _, p in ipairs(service.Players:GetPlayers()) do
        table.insert(list, p.DisplayName .. " (ID: " .. p.Name .. ")")
    end

    PlayerDropdown:Refresh(list, true)
end
UpdatePlayerList2()
service.Players.PlayerAdded:Connect(function()
    UpdatePlayerList2()
end)
service.Players.PlayerRemoving:Connect(function()
    UpdatePlayerList2()
end)

ObjectTab:AddDropdown({
	Name = "向きモード",
	Default = "Default",
	Options = {"Default", "Vertical", "Right", "Left"},
	Callback = function(v)
	Config.OrientationMode = v
	end
})

ObjectTab:AddSlider({
	Name = "ハートサイズ",
	Min = 2,
	Max = 50,
	Default = Config.Size,
	Callback = function(v)
	Config.Size = v
	end
})
ObjectTab:AddSlider({
	Name = "ハートの高さ",
	Min = -20,
	Max = 50,
	Default = Config.Height,
	Callback = function(v)
	Config.Height = v
	end
})
ObjectTab:AddSlider({
    Name = "オブジェクト数",
    Min = 6,
	Max = 100,
	Default = Config.ObjectCount,
    Callback = function(v)
        Config.ObjectCount = v
        if Config.HeartToggle then
        end
    end
})
ObjectTab:AddSlider({
	Name = "回転速度",
	Min = 0,
	Max = 10,
	Default = Config.RotationSpeed,
	Increment = 0.1,
	Callback = function(v)
	Config.RotationSpeed = v
	end
})
ObjectTab:AddSlider({
	Name = "パルス速度",
	Min = 0,
	Max = 10,
	Default = Config.PulseSpeed,
	Increment = 0.1,
	Callback = function(v)
	Config.PulseSpeed = v
	end
})

AutoTab:AddToggle({
    Name = "自動でガチャを回す",
    Default = false,
    Callback = function(Value)
        Config.AutoSlot = Value
    end
})

AutoTab:AddButton({
    Name = "Vfly",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/ZenoFTAP/scripts/main/VflyGUI.lua"))()
    end
})

AutoTab:AddSection({
    Name = "家関連"
})

local TimeLabel = AutoTab:AddLabel("ハウスタイム: 0")
local OwnerLabel = AutoTab:AddLabel("オーナー: ?")

AutoTab:AddToggle({
    Name = "プライベート時間",
    Default = false,
    Callback = function(Value)
        Config.autoHouseTP = Value

        if not Value and Config.isTeleported and Config.savedPosition then
            local hrp = HRP()
            if hrp then
                hrp.CFrame = Config.savedPosition
            end
            Config.isTeleported = false
            Config.savedPosition = nil
        end
    end
})

AutoTab:AddDropdown({
    Name = "家を選択",
    Default = Config.SelectedPlot,
    Options = {
        "Witch House",
        "Lumber House",
        "Common House",
        "American House",
        "Chinese House"
    },
    Callback = function(value)
        Config.SelectedPlot = value
    end
})

AutoTab:AddButton({
    Name = "所有権を取得",
    Callback = function()
        ClaimPlot()
    end
})

AutoTab:AddSection({
    Name = "バリア破壊"
})

AutoTab:AddToggle({
    Name = "自動バリア破壊 (ループ)",
    Default = false,
    Callback = function(Value)
        _G.BreakBarrierT = Value
        
        if Value then
            task.spawn(function()
                local function GetRoot(char)
                    return char and (char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso"))
                end

                while _G.BreakBarrierT do
                    local myRoot = GetRoot(localPlayer.Character)
                    if not myRoot then break end

                    local pos = myRoot.CFrame
                    local toy = SPNTOY("InstrumentWoodwindOcarina", myRoot.CFrame)

                    if not _G.BreakBarrierT then 
                        if toy then destroyToy(toy) end
                        break 
                    end

                    local args = {
                        [1] = toy,
                        [2] = localPlayer.Character
                    }
                    
                    if toy and toy:FindFirstChild("HoldPart") then
                        toy.HoldPart.HoldItemRemoteFunction:InvokeServer(unpack(args))
                    end
                    
                    task.wait(.05)
                    myRoot.CFrame = CFrame.new(304.06, 25.77, 488.54)
                    task.wait(.05)
                    destroyToy(toy)
                    task.wait(.05)
                    myRoot.CFrame = pos

                    for _, v in ipairs(service.Workspace.Plots:GetChildren()) do
                        local b = v:FindFirstChild("Barrier")
                        if b then
                            for _, p in ipairs(b:GetChildren()) do
                                p.CanCollide = false
                            end
                        end
                    end

                    task.wait(1)

                    OrionLib:MakeNotification({
                        Name = "Tsukuyomihub やおよろー！",
                        Content = "チェック中... (動かないでください。)",
                        Time = 5
                    })

                    local p = SPNTOY("FireExtinguisher", myRoot.CFrame)
                    if p and p:FindFirstChild("SoundPart") then
                        SetNetworkOwner:FireServer(p.SoundPart, myRoot.CFrame)
                        task.wait(.5)
                        p.SoundPart.CFrame = CFrame.new(304, 25, 488)
                        task.wait(.75)
                        
                        if p.Parent then
                            OrionLib:MakeNotification({
                                Name = "Tsukuyomihub やおよろー！",
                                Content = "⭕ バリア破壊に成功しました。",
                                Time = 5
                            })
                            destroyToy(p)
                            _G.BreakBarrierT = false
                            break 
                        else
                            OrionLib:MakeNotification({
                                Name = "Tsukuyomihub やおよろー！",
                                Content = "❌ 失敗。自動でまた行います。",
                                Time = 5
                            })
                            destroyToy(p)
                        end
                    end
                    
                    task.wait(1)
                end
            end)
        end
    end
})
local PlayerSelectDropdown = LoopTab:AddDropdown({
    Name = "プレイヤー選択",
    Default = {},
    Options = GetPlayerListForDropdown(),
    MultiSelect = true,
    Callback = function(Values)
        Config.PlayerList = {}
        
        for _, fullText in ipairs(Values) do
            local targetID = fullText:match("%(ID: (.+)%)")
            if targetID then
                table.insert(Config.PlayerList, targetID)
            end
        end

    end
})

local function RefreshDropdown()
    PlayerSelectDropdown:Refresh(GetPlayerListForDropdown(), true)
end

service.Players.PlayerAdded:Connect(RefreshDropdown)
service.Players.PlayerRemoving:Connect(function()
    task.wait()
    RefreshDropdown()
end)

LoopTab:AddSection({
	Name = "ループ"
})

LoopTab:AddToggle({
    Name = "ループキル",
    Callback = function(state)
        _G.LoopKill = state
        if state then
            while _G.LoopKill do
                Config.SavedCFrame = PLCF()
                for _, name in pairs(Config.PlayerList) do
                    local target = service.Players:FindFirstChild(name)
                    if IsValidTarget(target) then
                        local root = target.Character:FindFirstChild("HumanoidRootPart")
                        local hum = target.Character:FindFirstChild("Humanoid")
                        
                        if root and hum then
                            for _ = 0, 50 do
                                NoclipON()
                                ROS(root)
                                
                                if not IsValidTarget(target) or not _G.LoopKill or (CNWOSHIPOPl(target) or root.AssemblyLinearVelocity.Magnitude > 500) then
                                    DestroyGrabLine:FireServer(root)
                                    CSV(root)
                                    break
                                end
                                
                                task.wait()
                                if root.Position.Y <= -12 then
                                    TPPlayer(CFrame.new(root.Position + Vector3.new(0, 5, -15)))
                                else
                                    TPPlayer(CFrame.new(root.Position + Vector3.new(0, -10, -10)))
                                end
                                
                                hum.BreakJointsOnDeath = false
                                hum:ChangeState(Enum.HumanoidStateType.Dead)
                                hum.Jump = true
                            end
                        end
                    end
                end
                TPPlayer(Config.SavedCFrame)
                task.wait(0.2)
            end
            NoclipOFF()
            TPPlayer(Config.SavedCFrame)
        end
    end,
})

LoopTab:AddToggle({
    Name = "ループキック",
    Default = false,
    Callback = function(state)
        _G.LoopKickOwnership = state
        if state then
            while _G.LoopKickOwnership do
                Config.SavedCFrame = PLCF()
                for _, name in pairs(Config.PlayerList) do
                    local target = service.Players:FindFirstChild(name)
                    if IsValidTarget(target) then
                        local root = target.Character:FindFirstChild("HumanoidRootPart")
                        if root then
                            for _ = 0, 50 do
                                NoclipON()
                                ROS(root)
                                
                                if not IsValidTarget(target) or not _G.LoopKickOwnership or (CNWOSHIPOPl(target) or root.AssemblyLinearVelocity.Magnitude > 500) then
                                    DestroyGrabLine:FireServer(root)
                                    task.wait()
                                    CSV(root)
                                    break
                                end
                                
                                task.wait()
                                if root.Position.Y <= -12 then
                                    TPPlayer(CFrame.new(root.Position + Vector3.new(0, 5, -15)))
                                else
                                    TPPlayer(CFrame.new(root.Position + Vector3.new(0, -10, -10)))
                                end
                            end
                        end
                    end
                end
                TPPlayer(Config.SavedCFrame)
                task.wait(0.2)
            end
            NoclipOFF()
            TPPlayer(Config.SavedCFrame)
        end
    end,
})

MiscTab:AddToggle({
    Name = "キルオール",
    Default = false,
    Callback = function(state)
        _G.KillAll = state
        if state then
            while _G.KillAll do
                local oldPos = PLCF()
                for _, target in pairs(service.Players:GetPlayers()) do
                    if CanAttack(target) then
                        local root = target.Character.HumanoidRootPart
                        local hum = target.Character:FindFirstChildOfClass("Humanoid")

                        for _ = 0, 50 do
                            NoclipON()
                            ROS(root)

                            if not CanAttack(target) or not _G.KillAll or CNWOSHIPOPl(target) or root.AssemblyLinearVelocity.Magnitude > 500 then
                                CSV(root)
                                DestroyGrabLine:FireServer(root)
                                break
                            end
                            
                            task.wait()
                            local offset = (root.Position.Y <= -12) and Vector3.new(0, 5, -15) or Vector3.new(0, -10, -10)
                            TPPlayer(CFrame.new(root.Position + offset))
                            
                            hum.BreakJointsOnDeath = false
                            hum:ChangeState(Enum.HumanoidStateType.Dead)
                            hum.Jump, hum.Sit = true, false
                        end
                    end
                end
                TPPlayer(oldPos)
                task.wait(0.2)
            end
            NoclipOFF()
        end
    end
})

MiscTab:AddToggle({
    Name = "キックオール",
    Default = false,
    Callback = function(state)
        _G.KickAll = state
        if state then
            while _G.KickAll do
                local oldPos = PLCF()
                for _, target in pairs(service.Players:GetPlayers()) do
                    if CanAttack(target) then
                        local root = target.Character.HumanoidRootPart
                        for _ = 0, 50 do
                            NoclipON()
                            ROS(root)
                            if not CanAttack(target) or not _G.KickAll or CNWOSHIPOPl(target) or root.AssemblyLinearVelocity.Magnitude > 500 then
                                CSV(root)
                                DestroyGrabLine:FireServer(root)
                                break
                            end
                            task.wait()
                            local offset = (root.Position.Y <= -12) and Vector3.new(0, 5, -15) or Vector3.new(0, -10, -10)
                            TPPlayer(CFrame.new(root.Position + offset))
                        end
                    end
                end
                TPPlayer(oldPos)
                task.wait(0.2)
            end
            NoclipOFF()
        end
    end
})

MiscTab:AddToggle({
    Name = "ブリングオール",
    Default = false,
    Callback = function(enabled)
        Config.Active = enabled

        local function FreezeCamera()
            local Camera = service.Workspace.CurrentCamera
            Config.CamBlock.Anchored = true
            Config.CamBlock.CanCollide = false
            Config.CamBlock.Transparency = 1
            Config.CamBlock.CanQuery = false
            Config.CamBlock.Size = Vector3.new(10, 10, 10)
            Config.CamBlock.CFrame = Config.SavedCamCFrame
            Config.CamBlock.Parent = service.Workspace
            Camera.CameraType = Enum.CameraType.Scriptable
            Camera.CFrame = Config.SavedCamCFrame
        end

        local function UnfreezeCamera()
            Config.CamBlock.Parent = nil
            local Camera = service.Workspace.CurrentCamera
            Camera.CameraType = Enum.CameraType.Custom
            if Config.SavedCamCFrame then
                Camera.CFrame = Config.SavedCamCFrame
            end
            Config.SavedCamCFrame = nil
        end

        local function DisableCollision(model)
            for _, part in pairs(model:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end

        local function IsInPlot(player)
            local plotItems = service.Workspace:FindFirstChild("PlotItems")
            local playersInPlots = plotItems and plotItems:FindFirstChild("PlayersInPlots")
            return playersInPlots and playersInPlots:FindFirstChild(player.Name)
        end

        local function RefreshQueue()
            Config.Queue = {}
            for _, player in pairs(service.Players:GetPlayers()) do
                if player ~= localPlayer and player.Character and not whiteListToggle(player) then
                    if not IsInPlot(player) then
                        local root = player.Character:FindFirstChild("HumanoidRootPart")
                        if root and (root.Position - (Config.BasePos or Vector3.zero)).Magnitude > Config.ALLRadius then
                            table.insert(Config.Queue, player)
                        end
                    end
                end
            end
        end

        local function ProcessNext()
            if #Config.Queue == 0 then
                RefreshQueue()
                if #Config.Queue == 0 then return end
            end

            local target = table.remove(Config.Queue, 1)
            if target and target.Character then
                local targetRoot = target.Character:FindFirstChild("HumanoidRootPart")
                local targetHead = target.Character:FindFirstChild("Head")
                local myRoot = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")

                if targetRoot and targetHead and myRoot then
                    localPlayer.Character:PivotTo(targetRoot.CFrame * CFrame.new(0, -2, 0))
                    DisableCollision(localPlayer.Character)

                    local attempts = 0
                    repeat
                        SetNetworkOwner:FireServer(targetRoot, myRoot.CFrame)
                        task.wait(0.10)
                        attempts = attempts + 1
                    until attempts > 20 or (targetHead:FindFirstChild("PartOwner") and targetHead.PartOwner.Value == localPlayer.Name) or not Config.Active

                    if Config.Active and targetHead:FindFirstChild("PartOwner") and targetHead.PartOwner.Value == localPlayer.Name then
                        targetRoot.CFrame = CFrame.new(Config.BasePos)
                        targetRoot.Position = Config.BasePos
                        targetRoot.AssemblyLinearVelocity = Vector3.zero
                        task.wait(0.8)
                    end
                end
            end
        end

        if enabled then
            local root = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
            if root then
                Config.BasePos = root.Position
                Config.SavedCamCFrame = service.Workspace.CurrentCamera.CFrame
                RefreshQueue()
                FreezeCamera()
                
                Config.Connection = service.RunService.Heartbeat:Connect(function()
                    if Config.Active then
                        ProcessNext()
                        if Config.SavedCamCFrame then
                            local cam = service.Workspace.CurrentCamera
                            cam.CameraType = Enum.CameraType.Scriptable
                            cam.CFrame = Config.SavedCamCFrame
                            Config.CamBlock.CFrame = Config.SavedCamCFrame
                            Config.CamBlock.Parent = service.Workspace
                        end
                    end
                end)
            end
        else
            if Config.Connection then
                Config.Connection:Disconnect()
                Config.Connection = nil
            end
            UnfreezeCamera()
            
            local root = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
            if root and Config.BasePos then
                root.AssemblyLinearVelocity = Vector3.zero
                root.CFrame = CFrame.new(Config.BasePos)
            end
        end
    end
})

MiscTab:AddToggle({
    Name = "すべてのアイテムを吸い込む",
    Default = false,
    Callback = function(enabled)
        Config.GrabALLitemT = enabled
        
        local function ProcessFolder(folder)
            local targetItems = {}
            
            for _, item in pairs(folder:GetDescendants()) do
                if item:IsA("Model") and item:FindFirstChild("HoldPart") then
                    if item.HoldPart:FindFirstChild("HoldItemRemoteFunction") then
                        table.insert(targetItems, item)
                    end
                end
            end

            for i = #targetItems, 1, -1 do
                if not Config.GrabALLitemT then break end
                
                local model = targetItems[i]
                local holdPart = model:FindFirstChild("HoldPart")
                
                if holdPart then
                    local holdRemote = holdPart:FindFirstChild("HoldItemRemoteFunction")
                    local dropRemote = holdPart:FindFirstChild("DropItemRemoteFunction")
                    
                    if holdRemote then
                        pcall(function()
                            holdRemote:InvokeServer(model, localPlayer.Character)
                        end)
                    end
                    
                    if dropRemote then
                        pcall(function()
                            dropRemote:InvokeServer(model, Config.drop, Vector3.new())
                        end)
                    end
                end
                
                table.remove(targetItems, i)
                task.wait(0.2)
            end
        end

        if enabled then
            task.spawn(function()
                while Config.GrabALLitemT do
                    for _, child in pairs(service.Workspace:GetChildren()) do
                        if child:IsA("Folder") and child.Name:find("SpawnedInToys") then
                            ProcessFolder(child)
                        end
                    end

                    local plotItems = service.Workspace:FindFirstChild("PlotItems")
                    if plotItems then
                        for _, folder in pairs(plotItems:GetChildren()) do
                            if folder:IsA("Folder") then
                                ProcessFolder(folder)
                            end
                        end
                    end
                    
                    task.wait(1)
                end
            end)
        end
    end
})

MiscTab:AddToggle({
    Name = "フレンドを対象外にする",
    Default = false,
    Callback = function(state)
        Config.WhitelistFriends2 = state
    end
})

MiscTab:AddSection({
	Name = "チャット"
})

MiscTab:AddToggle({
    Name = "チャットタイピング無効化",
    Default = false,
    Callback = function(state)
        local char = localPlayer.Character or localPlayer.CharacterAdded:Wait()
        local chatTyping = char:FindFirstChild("ChatTyping")
        if chatTyping then
            chatTyping.Disabled = state
        end
    end
})

MiscTab:AddSection({
	Name = "R18"
})

MiscTab:AddToggle({
    Name = "オナニー",
    Default = false,
    Callback = function(on)
        playJerkOffActive = on
        if on then
            local char = localPlayer.Character or localPlayer.CharacterAdded:Wait()
            local hum = char:FindFirstChildOfClass("Humanoid")
            if not hum then return end

            local animator = hum:FindFirstChildOfClass("Animator")
            if not animator then
                animator = Instance.new("Animator")
                animator.Parent = hum
            end

            local jerkOffAnimId = "rbxassetid://168268306"
            local anim = Instance.new("Animation")
            anim.AnimationId = jerkOffAnimId

            Config.jerkOffAnimTrack = animator:LoadAnimation(anim)
            Config.jerkOffAnimTrack.Priority = Enum.AnimationPriority.Action
            Config.jerkOffAnimTrack:Play()

            task.spawn(function()
                while playJerkOffActive do
                    task.wait(0.1)
                    if Config.jerkOffAnimTrack and Config.jerkOffAnimTrack.IsPlaying then
                        Config.jerkOffAnimTrack.TimePosition = 0.3
                    end
                end
            end)
        else
            if Config.jerkOffAnimTrack then
                Config.jerkOffAnimTrack:Stop()
                Config.jerkOffAnimTrack = nil
            end
        end
    end
})

MiscTab:AddButton({
    Name = "ちんこ",
    Callback = function()
        pcall(function()

            local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()

            local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
            local torso = character:WaitForChild("Torso")

            local spawnPosition = humanoidRootPart.Position

            SpawnToyRemoteFunction:InvokeServer(
                "ToolPencil",
                CFrame.new(spawnPosition),
                Vector3.new(0, 0, 0)
            )

            task.wait(0.3)

            if not SpawnedInToys then return end

            local toolPencil = SpawnedInToys:FindFirstChild("ToolPencil")
            if not toolPencil then return end

            local stickyPart = toolPencil:FindFirstChild("StickyPart")
            local soundPart = toolPencil:FindFirstChild("SoundPart")
            if not (stickyPart and soundPart) then return end

            pcall(function()
                SetNetworkOwner:FireServer(
                    soundPart,
                    torso.CFrame
                )
            end)

            StickyPartEvent:FireServer(
                stickyPart,
                torso,
                CFrame.new(
                    0, -1, 0,
                    -1, 0, -8.74227766e-08,
                    0, 1, 0,
                    8.74227766e-08, 0, -1
                )
            )

        end)
    end
})

VisualTab:AddLabel("偽コイン")

VisualTab:AddTextbox({
    Name = "偽コインの枚数",
    Default = "",
    TextDisappear = false,
    Callback = function(text)
        skolko = text
    end
})

VisualTab:AddButton({
    Name = "取得",
    Callback = function()
        local coinAmount = tonumber(skolko) or 0
        local playerGui = game.Players.LocalPlayer.PlayerGui
        local coinText = playerGui.MenuGui.TopRight.CoinsFrame.CoinsDisplay.Coins
        if coinText then
            coinText.Text = tostring(coinAmount)
        end
    end
})

WorldTab:AddButton({
    Name = "再参加",
    Callback = function()
        pcall(function()
            service.TeleportService:Teleport(game.PlaceId, service.Players.LocalPlayer)
        end)
    end
})

WorldTab:AddLabel("明るさ")

WorldTab:AddToggle({
    Name = "明るさを適用",
    Default = Config.BrightnessToggle,
    Callback = function(Value)
        Config.BrightnessToggle = Value
        if not Config.OriginalBrightness then
            Config.OriginalBrightness = service.Lighting.Brightness
        end
        if Value then
            service.Lighting.Brightness = Config.BrightnessLevel
        else
            service.Lighting.Brightness = Config.OriginalBrightness
        end
    end
})

WorldTab:AddSlider({
    Name = "明るさ",
    Min = 0,
    Max = 10,
    Default = Config.BrightnessLevel,
    Increment = 0.1,
    Callback = function(Value)
        Config.BrightnessLevel = Value
        if Config.BrightnessToggle then
            service.Lighting.Brightness = Value
        end
    end
})

WorldTab:AddLabel("明るさをmax")

WorldTab:AddToggle({
    Name = "明るさをmax",
    Default = Config.FullBright,
    Callback = function(Value)
        Config.FullBright = Value
        if Value then
            Config.OriginalBrightness = service.Lighting.Brightness
            Config.OriginalFogEnd = service.Lighting.FogEnd
            Config.OriginalShadows = service.Lighting.GlobalShadows
            Config.OriginalAmbient = service.Lighting.Ambient
            Config.OriginalOutdoorAmbient = service.Lighting.OutdoorAmbient

            service.Lighting.Brightness = 5
            service.Lighting.ClockTime = 14
            service.Lighting.FogEnd = 100000
            service.Lighting.GlobalShadows = false
            service.Lighting.Ambient = Color3.fromRGB(255,255,255)
            service.Lighting.OutdoorAmbient = Color3.fromRGB(255,255,255)
        else
            service.Lighting.Brightness = Config.OriginalBrightness or service.Lighting.Brightness
            service.Lighting.FogEnd = Config.OriginalFogEnd or service.Lighting.FogEnd
            service.Lighting.GlobalShadows = Config.OriginalShadows or service.Lighting.GlobalShadows
            service.Lighting.Ambient = Config.OriginalAmbient or service.Lighting.Ambient
            service.Lighting.OutdoorAmbient = Config.OriginalOutdoorAmbient or service.Lighting.OutdoorAmbient
        end
    end
})

WorldTab:AddLabel("影")

WorldTab:AddToggle({
    Name = "影を適用",
    Default = Config.ShadowToggle,
    Callback = function(Value)
        Config.ShadowToggle = Value
        if not Config.OriginalShadows then
            Config.OriginalShadows = service.Lighting.GlobalShadows
        end
        service.Lighting.GlobalShadows = Value
    end
})

WorldTab:AddSlider({
    Name = "影",
    Min = 0,
    Max = 2,
    Default = Config.ShadowLevel,
    Increment = 0.1,
    Callback = function(Value)
        Config.ShadowLevel = Value
        service.Lighting.ShadowSoftness = Value
    end
})

WorldTab:AddLabel("霧")

WorldTab:AddToggle({
    Name = "霧を適用",
    Default = Config.FogToggle,
    Callback = function(Value)
        Config.FogToggle = Value
        if not Config.OriginalFogEnd then
            Config.OriginalFogEnd = service.Lighting.FogEnd
        end
        if Value then
            service.Lighting.FogEnd = Config.FogLevel
        else
            service.Lighting.FogEnd = Config.OriginalFogEnd
        end
    end
})

WorldTab:AddSlider({
    Name = "霧",
    Min = 0,
    Max = 1000,
    Default = Config.FogLevel,
    Increment = 10,
    Callback = function(Value)
        Config.FogLevel = Value
        if Config.FogToggle then
            service.Lighting.FogEnd = Value
        end
    end
})

WorldTab:AddLabel("時間帯")

WorldTab:AddToggle({
    Name = "時間帯を適用",
    Default = false,
    Callback = function(Value)
        _G.TimeToggle = Value
    end
})

WorldTab:AddDropdown({
    Name = "時間帯を選択",
    Default = "朝",
    Options = {"朝", "昼", "夕方", "夜"},
    Callback = function(Value)
        if _G.TimeToggle then
            if Value == "朝" then
                game:GetService("Lighting").ClockTime = 6
            elseif Value == "昼" then
                game:GetService("Lighting").ClockTime = 12
            elseif Value == "夕方" then
                game:GetService("Lighting").ClockTime = 18
            elseif Value == "夜" then
                game:GetService("Lighting").ClockTime = 0
            end
        end
    end
})

service.Players.PlayerAdded:Connect(function()
    local list = {}
    for _, p in ipairs(service.Players:GetPlayers()) do
        table.insert(list, p.DisplayName .. " (ID: " .. p.Name .. ")")
    end
    PlayerDropdown:Refresh(list)
end)

service.Players.PlayerRemoving:Connect(function()
    local list = {}
    for _, p in ipairs(service.Players:GetPlayers()) do
        table.insert(list, p.DisplayName .. " (ID: " .. p.Name .. ")")
    end
    PlayerDropdown:Refresh(list)
end)

task.spawn(function()
    while task.wait(1) do
        local plot = GetPlot()
        if not plot then continue end

        local timeObj = plot:FindFirstChild("TimeRemainingNum", true)

        if timeObj and timeObj:IsA("IntValue") then
            local timeValue = timeObj.Value

            if TimeLabel then
                TimeLabel:Set("ハウスタイム: " .. timeValue)
            end

            local hrp = HRP()
            if hrp then
                if Config.autoHouseTP then
                    if timeValue <= 15 then
                        if not Config.isTeleported then
                            Config.savedPosition = hrp.CFrame
                            Config.isTeleported = true

                            if plot:FindFirstChild("Teleport") then
                                hrp.CFrame = plot.Teleport.CFrame
                            else
                                hrp.CFrame = plot:GetModelCFrame()
                            end
                        end
                    elseif timeValue > 15 then
                        if Config.isTeleported and Config.savedPosition then
                            hrp.CFrame = Config.savedPosition
                            Config.isTeleported = false
                            Config.savedPosition = nil
                        end
                    end
                end
            end
        end

        local ownerName = PlotOwner()
        if ownerName and ownerName ~= "" then
            OwnerLabel:Set("オーナー: " .. ownerName)
        else
            OwnerLabel:Set("オーナー: ?")
        end
    end
end)
service.UserInputService.InputChanged:Connect(function(input)

	if not Config.ZoomBind then return end
	if input.UserInputType ~= Enum.UserInputType.MouseWheel then return end

	local delta = input.Position.Z

	if delta > 0 then
		Config.ZFOV = math.max(Config.MIZ, Config.ZFOV - Config.ZSP)
	elseif delta < 0 then
		Config.ZFOV = math.min(Config.MXZ, Config.ZFOV + Config.ZSP)
	end

	Camera.FieldOfView = Config.ZFOV

end)
service.UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.Q then
        playJerkOffActive = not playJerkOffActive
        local on = playJerkOffActive
        if on then
            local plr = service.Players.LocalPlayer
            local char = plr.Character or plr.CharacterAdded:Wait()
            local hum = char:FindFirstChildOfClass("Humanoid")
            if not hum then return end

            local animator = hum:FindFirstChildOfClass("Animator")
            if not animator then
                animator = Instance.new("Animator")
                animator.Parent = hum
            end

            local jerkOffAnimId = "rbxassetid://168268306"
            local anim = Instance.new("Animation")
            anim.AnimationId = jerkOffAnimId

            Config.jerkOffAnimTrack = animator:LoadAnimation(anim)
            Config.jerkOffAnimTrack.Priority = Enum.AnimationPriority.Action
            Config.jerkOffAnimTrack:Play()

            task.spawn(function()
                while playJerkOffActive do
                    task.wait(0.1)
                    if Config.jerkOffAnimTrack and Config.jerkOffAnimTrack.IsPlaying then
                        Config.jerkOffAnimTrack.TimePosition = 0.3
                    end
                end
            end)
        else
            if Config.jerkOffAnimTrack then
                Config.jerkOffAnimTrack:Stop()
                Config.jerkOffAnimTrack = nil
            end
        end
    end
end)

Workspace.ChildAdded:Connect(GrabParts)

-- ====================================================================
-- さくらhub 統合タブ群
-- ====================================================================

-- オブジェクト設定タブ
local ObjectIDTab = Window:MakeTab({Name = "オブジェクト設定", Icon = "rbxassetid://4483362458", PremiumOnly = false})
ObjectIDTab:AddLabel("使用オブジェクトID を選択 (28種類)")
for i = 1, #ObjectIDConfig.AvailableObjects do
    local id = ObjectIDConfig.AvailableObjects[i]
    ObjectIDTab:AddButton({Name = id, Callback = function() changeObjectID(id) end})
end

-- フェザー[羽]タブ
local FeatherTab = Window:MakeTab({Name = "フェザー[羽]", Icon = "rbxassetid://4483362458", PremiumOnly = false})
FeatherTab:AddToggle({Name = "フェザー起動", Default = false, Callback = toggleFeather})
FeatherTab:AddSection({Name = "配置設定"})
FeatherTab:AddSlider({Name = "最大数", Min = 2, Max = 100, Default = 20, Increment = 2, Callback = function(v) FeatherConfig.maxSparklers = v end})
FeatherTab:AddSlider({Name = "間隔", Min = 1, Max = 20, Default = 3, Increment = 0.5, Callback = function(v) FeatherConfig.spacing = v end})
FeatherTab:AddSlider({Name = "高さオフセット", Min = -10, Max = 30, Default = 2, Increment = 0.5, Callback = function(v) FeatherConfig.heightOffset = v end})
FeatherTab:AddSlider({Name = "背面オフセット", Min = 0, Max = 30, Default = 3, Increment = 0.5, Callback = function(v) FeatherConfig.backwardOffset = v end})
FeatherTab:AddSection({Name = "角度・動き"})
FeatherTab:AddSlider({Name = "傾き角度", Min = 0, Max = 90, Default = 45, Increment = 5, Callback = function(v) FeatherConfig.tiltAngle = v end})
FeatherTab:AddSlider({Name = "上下動速度", Min = 0, Max = 20, Default = 2, Increment = 0.5, Callback = function(v) FeatherConfig.waveSpeed = v end})
FeatherTab:AddSlider({Name = "振幅", Min = 0, Max = 20, Default = 1, Increment = 0.5, Callback = function(v) FeatherConfig.baseAmplitude = v end})

-- 魔法陣タブ
local MagicCircleTab = Window:MakeTab({Name = "魔法陣", Icon = "rbxassetid://4483362458", PremiumOnly = false})
MagicCircleTab:AddToggle({Name = "魔法陣起動", Default = false, Callback = toggleMagicCircle})
MagicCircleTab:AddSection({Name = "基本設定"})
MagicCircleTab:AddDropdown({Name = "シンボルタイプ", Default = "Ring", Options = {"Ring","Circle","Hexagram"}, Callback = function(v) MagicCircleConfig.SymbolType = v end})
MagicCircleTab:AddToggle({Name = "発光効果", Default = true, Callback = function(v) MagicCircleConfig.GlowEffect = v end})
MagicCircleTab:AddSlider({Name = "高さ", Min = 0, Max = 50, Default = 5, Increment = 1, Callback = function(v) MagicCircleConfig.Height = v end})
MagicCircleTab:AddSlider({Name = "直径", Min = 5, Max = 50, Default = 5, Increment = 1, Callback = function(v) MagicCircleConfig.Diameter = v end})
MagicCircleTab:AddSlider({Name = "オブジェクト数", Min = 3, Max = 100, Default = 10, Increment = 1, Callback = function(v) MagicCircleConfig.ObjectCount = v; if MagicCircleConfig.Enabled then rescanMagicCircle() end end})
MagicCircleTab:AddSlider({Name = "回転速度", Min = 1, Max = 100, Default = 20, Increment = 1, Callback = function(v) MagicCircleConfig.RotationSpeed = v end})
MagicCircleTab:AddButton({Name = "再スキャン", Callback = function() if MagicCircleConfig.Enabled then rescanMagicCircle() end end})

-- ♡ハート//タブ
local HeartTab2 = Window:MakeTab({Name = "♡ハート//", Icon = "rbxassetid://4483362458", PremiumOnly = false})
HeartTab2:AddToggle({Name = "ハート起動", Default = false, Callback = toggleHeart2})
HeartTab2:AddToggle({Name = "プレイヤー追従", Default = true, Callback = function(v) HeartConfig2.FollowPlayer = v end})
HeartTab2:AddSection({Name = "サイズ・数"})
HeartTab2:AddSlider({Name = "ハートサイズ", Min = 2, Max = 50, Default = 5, Increment = 1, Callback = function(v) HeartConfig2.Size = v end})
HeartTab2:AddSlider({Name = "高さ", Min = 0, Max = 50, Default = 5, Increment = 1, Callback = function(v) HeartConfig2.Height = v end})
HeartTab2:AddSlider({Name = "オブジェクト数", Min = 6, Max = 100, Default = 12, Increment = 2, Callback = function(v) HeartConfig2.ObjectCount = v; if HeartConfig2.Enabled then toggleHeart2(false); task.wait(0.1); toggleHeart2(true) end end})
HeartTab2:AddSection({Name = "動き"})
HeartTab2:AddSlider({Name = "回転速度", Min = 0, Max = 10, Default = 1, Increment = 0.1, Callback = function(v) HeartConfig2.RotationSpeed = v end})
HeartTab2:AddSlider({Name = "脈動速度", Min = 0, Max = 10, Default = 2, Increment = 0.1, Callback = function(v) HeartConfig2.PulseSpeed = v end})
HeartTab2:AddSlider({Name = "脈動振幅", Min = 0, Max = 10, Default = 0.5, Increment = 0.1, Callback = function(v) HeartConfig2.PulseAmplitude = v end})

-- おっきぃ♡タブ
local BigHeartTab2 = Window:MakeTab({Name = "おっきぃ♡", Icon = "rbxassetid://4483362458", PremiumOnly = false})
BigHeartTab2:AddToggle({Name = "おっきぃ♡起動", Default = false, Callback = toggleBigHeart})
BigHeartTab2:AddSection({Name = "サイズ"})
BigHeartTab2:AddSlider({Name = "基本サイズ", Min = 5, Max = 50, Default = 10, Increment = 1, Callback = function(v) BigHeartConfig.Size = v end})
BigHeartTab2:AddSlider({Name = "拡大率", Min = 1, Max = 10, Default = 2, Increment = 0.1, Callback = function(v) BigHeartConfig.HeartScale = v end})
BigHeartTab2:AddSlider({Name = "縦引き伸ばし", Min = 1, Max = 5, Default = 1.2, Increment = 0.1, Callback = function(v) BigHeartConfig.VerticalStretch = v end})
BigHeartTab2:AddSlider({Name = "高さ", Min = 5, Max = 50, Default = 8, Increment = 1, Callback = function(v) BigHeartConfig.Height = v end})
BigHeartTab2:AddSlider({Name = "オブジェクト数", Min = 12, Max = 100, Default = 20, Increment = 2, Callback = function(v) BigHeartConfig.ObjectCount = v; if BigHeartConfig.Enabled then toggleBigHeart(false); task.wait(0.1); toggleBigHeart(true) end end})
BigHeartTab2:AddSection({Name = "動き"})
BigHeartTab2:AddSlider({Name = "回転速度", Min = 0, Max = 10, Default = 0.5, Increment = 0.5, Callback = function(v) BigHeartConfig.RotationSpeed = v end})
BigHeartTab2:AddSlider({Name = "脈動速度", Min = 0, Max = 10, Default = 1, Increment = 0.5, Callback = function(v) BigHeartConfig.PulseSpeed = v end})
BigHeartTab2:AddSlider({Name = "脈動振幅", Min = 0, Max = 10, Default = 1, Increment = 0.1, Callback = function(v) BigHeartConfig.PulseAmplitude = v end})

-- ダビデの星✡タブ
local StarOfDavidTab = Window:MakeTab({Name = "ダビデの星✡", Icon = "rbxassetid://4483362458", PremiumOnly = false})
StarOfDavidTab:AddToggle({Name = "ダビデの星起動", Default = false, Callback = toggleStarOfDavid})
StarOfDavidTab:AddSlider({Name = "サイズ", Min = 2, Max = 50, Default = 5, Increment = 1, Callback = function(v) StarOfDavidConfig.Size = v end})
StarOfDavidTab:AddSlider({Name = "高さ", Min = 0, Max = 50, Default = 5, Increment = 1, Callback = function(v) StarOfDavidConfig.Height = v end})
StarOfDavidTab:AddSlider({Name = "三角形の高さ", Min = 0, Max = 20, Default = 0.5, Increment = 0.1, Callback = function(v) StarOfDavidConfig.TriangleHeight = v end})
StarOfDavidTab:AddSlider({Name = "オブジェクト数", Min = 6, Max = 100, Default = 12, Increment = 2, Callback = function(v) StarOfDavidConfig.ObjectCount = v; if StarOfDavidConfig.Enabled then toggleStarOfDavid(false); task.wait(0.1); toggleStarOfDavid(true) end end})
StarOfDavidTab:AddSlider({Name = "回転速度", Min = 0, Max = 10, Default = 1, Increment = 0.1, Callback = function(v) StarOfDavidConfig.RotationSpeed = v end})
StarOfDavidTab:AddSlider({Name = "脈動速度", Min = 0, Max = 10, Default = 1.5, Increment = 0.1, Callback = function(v) StarOfDavidConfig.PulseSpeed = v end})

-- スター★タブ
local StarTab = Window:MakeTab({Name = "スター★", Icon = "rbxassetid://4483362458", PremiumOnly = false})
StarTab:AddToggle({Name = "スター起動", Default = false, Callback = toggleStar})
StarTab:AddSlider({Name = "外側半径", Min = 2, Max = 50, Default = 5, Increment = 1, Callback = function(v) StarConfig.OuterRadius = v end})
StarTab:AddSlider({Name = "内側半径", Min = 1, Max = 30, Default = 2, Increment = 1, Callback = function(v) StarConfig.InnerRadius = v end})
StarTab:AddSlider({Name = "高さ", Min = 0, Max = 50, Default = 5, Increment = 1, Callback = function(v) StarConfig.Height = v end})
StarTab:AddSlider({Name = "オブジェクト数", Min = 5, Max = 100, Default = 10, Increment = 1, Callback = function(v) StarConfig.ObjectCount = v; if StarConfig.Enabled then toggleStar(false); task.wait(0.1); toggleStar(true) end end})
StarTab:AddSlider({Name = "回転速度", Min = 0, Max = 10, Default = 1, Increment = 0.1, Callback = function(v) StarConfig.RotationSpeed = v end})
StarTab:AddSlider({Name = "きらめき速度", Min = 0, Max = 10, Default = 2, Increment = 0.1, Callback = function(v) StarConfig.TwinkleSpeed = v end})

-- スター2✫タブ
local Star2Tab = Window:MakeTab({Name = "スター2✫", Icon = "rbxassetid://4483362458", PremiumOnly = false})
Star2Tab:AddToggle({Name = "スター2起動", Default = false, Callback = toggleStar2})
Star2Tab:AddSlider({Name = "基本サイズ", Min = 5, Max = 30, Default = 15, Increment = 1, Callback = function(v) Star2Config.Size = v end})
Star2Tab:AddSlider({Name = "光線の長さ", Min = 1, Max = 10, Default = 3, Increment = 0.5, Callback = function(v) Star2Config.RayLength = v end})
Star2Tab:AddSlider({Name = "高さ", Min = 5, Max = 50, Default = 10, Increment = 1, Callback = function(v) Star2Config.Height = v end})
Star2Tab:AddSlider({Name = "光線の数", Min = 6, Max = 36, Default = 12, Increment = 2, Callback = function(v) Star2Config.RayCount = v; if Star2Config.Enabled then toggleStar2(false); task.wait(0.1); toggleStar2(true) end end})
Star2Tab:AddSlider({Name = "オブジェクト数", Min = 12, Max = 100, Default = 24, Increment = 4, Callback = function(v) Star2Config.ObjectCount = v; if Star2Config.Enabled then toggleStar2(false); task.wait(0.1); toggleStar2(true) end end})
Star2Tab:AddSlider({Name = "回転速度", Min = 0, Max = 30, Default = 5, Increment = 1, Callback = function(v) Star2Config.RotationSpeed = v end})
Star2Tab:AddSlider({Name = "脈動速度", Min = 0, Max = 20, Default = 8, Increment = 1, Callback = function(v) Star2Config.PulseSpeed = v end})
Star2Tab:AddSlider({Name = "脈動振幅", Min = 0, Max = 10, Default = 2, Increment = 0.2, Callback = function(v) Star2Config.PulseAmplitude = v end})
Star2Tab:AddSlider({Name = "ギザギザ速度", Min = 0, Max = 20, Default = 5, Increment = 0.5, Callback = function(v) Star2Config.JitterSpeed = v end})
Star2Tab:AddSlider({Name = "ギザギザ量", Min = 0, Max = 10, Default = 1, Increment = 0.1, Callback = function(v) Star2Config.JitterAmount = v end})

-- 球体◯タブ
local SphereTab = Window:MakeTab({Name = "球体◯", Icon = "rbxassetid://4483362458", PremiumOnly = false})
SphereTab:AddToggle({Name = "球体起動", Default = false, Callback = toggleSphere})
SphereTab:AddSlider({Name = "半径", Min = 2, Max = 50, Default = 5, Increment = 1, Callback = function(v) SphereConfig.Radius = v end})
SphereTab:AddSlider({Name = "基本高さ", Min = -20, Max = 20, Default = 0, Increment = 1, Callback = function(v) SphereConfig.BaseHeight = v end})
SphereTab:AddSlider({Name = "緯度線", Min = 2, Max = 16, Default = 3, Increment = 1, Callback = function(v) SphereConfig.Latitudes = v; if SphereConfig.Enabled then toggleSphere(false); task.wait(0.1); toggleSphere(true) end end})
SphereTab:AddSlider({Name = "経度線", Min = 4, Max = 24, Default = 6, Increment = 2, Callback = function(v) SphereConfig.Longitudes = v; if SphereConfig.Enabled then toggleSphere(false); task.wait(0.1); toggleSphere(true) end end})
SphereTab:AddSlider({Name = "オブジェクト数", Min = 8, Max = 100, Default = 20, Increment = 4, Callback = function(v) SphereConfig.ObjectCount = v; if SphereConfig.Enabled then toggleSphere(false); task.wait(0.1); toggleSphere(true) end end})
SphereTab:AddSlider({Name = "水平回転速度", Min = 0, Max = 10, Default = 2, Increment = 0.1, Callback = function(v) SphereConfig.HorizontalRotationSpeed = v end})
SphereTab:AddSlider({Name = "垂直回転速度", Min = 0, Max = 10, Default = 1, Increment = 0.1, Callback = function(v) SphereConfig.VerticalRotationSpeed = v end})
SphereTab:AddSlider({Name = "波速度", Min = 0, Max = 10, Default = 1, Increment = 0.1, Callback = function(v) SphereConfig.WaveSpeed = v end})
SphereTab:AddSlider({Name = "波振幅", Min = 0, Max = 10, Default = 0.3, Increment = 0.1, Callback = function(v) SphereConfig.WaveAmplitude = v end})
SphereTab:AddSlider({Name = "脈動速度", Min = 0, Max = 10, Default = 1, Increment = 0.1, Callback = function(v) SphereConfig.PulseSpeed = v end})
SphereTab:AddSlider({Name = "脈動振幅", Min = 0, Max = 5, Default = 0.5, Increment = 0.1, Callback = function(v) SphereConfig.PulseAmplitude = v end})

-- 観覧車タブ
local FerrisTab = Window:MakeTab({Name = "観覧車", Icon = "rbxassetid://4483362458", PremiumOnly = false})
FerrisTab:AddToggle({Name = "観覧車起動", Default = false, Callback = toggleFerrisWheel})
FerrisTab:AddToggle({Name = "縦方向円", Default = true, Callback = function(v) FerrisWheelConfig.VerticalCircle = v end})
FerrisTab:AddSection({Name = "固定方向"})
FerrisTab:AddToggle({Name = "固定方向を使用", Default = true, Callback = function(v) FerrisWheelConfig.FixedDirection = v end})
FerrisTab:AddSlider({Name = "固定ヨー角", Min = -180, Max = 180, Default = 0, Increment = 5, Callback = function(v) FerrisWheelConfig.FixedYaw = v end})
FerrisTab:AddSlider({Name = "固定ピッチ角", Min = -90, Max = 90, Default = 0, Increment = 5, Callback = function(v) FerrisWheelConfig.FixedPitch = v end})
FerrisTab:AddSlider({Name = "固定ロール角", Min = -180, Max = 180, Default = 0, Increment = 5, Callback = function(v) FerrisWheelConfig.FixedRoll = v end})
FerrisTab:AddSection({Name = "サイズ・動き"})
FerrisTab:AddSlider({Name = "半径", Min = 5, Max = 50, Default = 10, Increment = 1, Callback = function(v) FerrisWheelConfig.Radius = v end})
FerrisTab:AddSlider({Name = "中心高さ", Min = 5, Max = 50, Default = 15, Increment = 1, Callback = function(v) FerrisWheelConfig.Height = v end})
FerrisTab:AddSlider({Name = "オブジェクト数", Min = 6, Max = 100, Default = 12, Increment = 2, Callback = function(v) FerrisWheelConfig.ObjectCount = v; if FerrisWheelConfig.Enabled then toggleFerrisWheel(false); task.wait(0.1); toggleFerrisWheel(true) end end})
FerrisTab:AddSlider({Name = "回転速度", Min = 0, Max = 5, Default = 1, Increment = 0.1, Callback = function(v) FerrisWheelConfig.RotationSpeed = v end})
FerrisTab:AddSection({Name = "効果"})
FerrisTab:AddToggle({Name = "脈動効果", Default = false, Callback = function(v) FerrisWheelConfig.PulseEffect = v end})
FerrisTab:AddSlider({Name = "脈動速度", Min = 0, Max = 10, Default = 1, Increment = 0.1, Callback = function(v) FerrisWheelConfig.PulseSpeed = v end})
FerrisTab:AddSlider({Name = "脈動振幅", Min = 0, Max = 10, Default = 2, Increment = 0.2, Callback = function(v) FerrisWheelConfig.PulseAmplitude = v end})
FerrisTab:AddToggle({Name = "揺れ効果", Default = false, Callback = function(v) FerrisWheelConfig.SwingEffect = v end})
FerrisTab:AddSlider({Name = "揺れの大きさ", Min = 0, Max = 10, Default = 0.5, Increment = 0.1, Callback = function(v) FerrisWheelConfig.SwingAmount = v end})

-- N1: カオス・サークルタブ
local AnimN1Tab = Window:MakeTab({Name = "N1: カオス・サークル", Icon = "rbxassetid://4483362458", PremiumOnly = false})
AnimN1Tab:AddToggle({Name = "N1起動", Default = false, Callback = toggleAnimN1})
AnimN1Tab:AddSlider({Name = "半径", Min = 5, Max = 50, Default = 15, Increment = 1, Callback = function(v) AnimN1Config.Radius = v end})
AnimN1Tab:AddSlider({Name = "高さ", Min = 0, Max = 50, Default = 10, Increment = 1, Callback = function(v) AnimN1Config.Height = v end})
AnimN1Tab:AddSlider({Name = "オブジェクト数", Min = 10, Max = 100, Default = 50, Increment = 2, Callback = function(v) AnimN1Config.ObjectCount = v; if AnimN1Config.Enabled then toggleAnimN1(false); task.wait(0.1); toggleAnimN1(true) end end})
AnimN1Tab:AddSlider({Name = "回転速度", Min = 0, Max = 50, Default = 20, Increment = 1, Callback = function(v) AnimN1Config.RotationSpeed = v end})
AnimN1Tab:AddSlider({Name = "脈動速度", Min = 0, Max = 20, Default = 5, Increment = 0.5, Callback = function(v) AnimN1Config.PulseSpeed = v end})
AnimN1Tab:AddSlider({Name = "脈動量", Min = 0, Max = 30, Default = 10, Increment = 1, Callback = function(v) AnimN1Config.PulseAmount = v end})

-- N2: トルネード・スパイラルタブ
local AnimN2Tab = Window:MakeTab({Name = "N2: トルネード・スパイラル", Icon = "rbxassetid://4483362458", PremiumOnly = false})
AnimN2Tab:AddToggle({Name = "N2起動", Default = false, Callback = toggleAnimN2})
AnimN2Tab:AddSlider({Name = "半径", Min = 3, Max = 30, Default = 8, Increment = 1, Callback = function(v) AnimN2Config.Radius = v end})
AnimN2Tab:AddSlider({Name = "最低高さ", Min = 0, Max = 20, Default = 5, Increment = 1, Callback = function(v) AnimN2Config.BaseHeight = v end})
AnimN2Tab:AddSlider({Name = "最高高さ", Min = 10, Max = 100, Default = 30, Increment = 1, Callback = function(v) AnimN2Config.TopHeight = v end})
AnimN2Tab:AddSlider({Name = "オブジェクト数", Min = 10, Max = 100, Default = 60, Increment = 2, Callback = function(v) AnimN2Config.ObjectCount = v; if AnimN2Config.Enabled then toggleAnimN2(false); task.wait(0.1); toggleAnimN2(true) end end})
AnimN2Tab:AddSlider({Name = "回転速度", Min = 0, Max = 50, Default = 15, Increment = 1, Callback = function(v) AnimN2Config.RotationSpeed = v end})
AnimN2Tab:AddSlider({Name = "上昇速度", Min = 0, Max = 10, Default = 2, Increment = 0.2, Callback = function(v) AnimN2Config.RiseSpeed = v end})
AnimN2Tab:AddSlider({Name = "カオス要素", Min = 0, Max = 10, Default = 3, Increment = 0.2, Callback = function(v) AnimN2Config.ChaosFactor = v end})

-- N3: ハイパー・エクスプロージョンタブ
local AnimN3Tab = Window:MakeTab({Name = "N3: ハイパー・エクスプロージョン", Icon = "rbxassetid://4483362458", PremiumOnly = false})
AnimN3Tab:AddToggle({Name = "N3起動", Default = false, Callback = toggleAnimN3})
AnimN3Tab:AddSlider({Name = "基本高さ", Min = 0, Max = 50, Default = 8, Increment = 1, Callback = function(v) AnimN3Config.Height = v end})
AnimN3Tab:AddSlider({Name = "爆発半径", Min = 5, Max = 100, Default = 25, Increment = 1, Callback = function(v) AnimN3Config.ExplosionRadius = v end})
AnimN3Tab:AddSlider({Name = "オブジェクト数", Min = 10, Max = 100, Default = 80, Increment = 2, Callback = function(v) AnimN3Config.ObjectCount = v; if AnimN3Config.Enabled then toggleAnimN3(false); task.wait(0.1); toggleAnimN3(true) end end})
AnimN3Tab:AddSlider({Name = "サイクル速度", Min = 0, Max = 5, Default = 2, Increment = 0.1, Callback = function(v) AnimN3Config.CycleSpeed = v end})
AnimN3Tab:AddSlider({Name = "爆発速度", Min = 1, Max = 20, Default = 10, Increment = 0.5, Callback = function(v) AnimN3Config.ExplosionSpeed = v end})
AnimN3Tab:AddSlider({Name = "ランダム性", Min = 0, Max = 20, Default = 5, Increment = 0.5, Callback = function(v) AnimN3Config.Randomness = v end})

-- アニメーション全停止ボタン（共通）
AnimN3Tab:AddButton({Name = "全アニメーション停止", Callback = stopAllAnimations})

-- スクリプトhubタブ
local ScriptHubTab = Window:MakeTab({Name = "スクリプトhub", Icon = "rbxassetid://101768155599700", PremiumOnly = false})
ScriptHubTab:AddSection({Name = "外部スクリプト"})
ScriptHubTab:AddButton({Name = "シェーダー", Callback = function() executeScript("https://rawscripts.net/raw/Universal-Script-Shader-77482","シェーダー") end})
ScriptHubTab:AddButton({Name = "空変え", Callback = function() executeScript("https://rawscripts.net/raw/Universal-Script-SkyBoxinjectHUB-80671","空変え") end})
ScriptHubTab:AddButton({Name = "テトリス", Callback = function() executeScript("https://rawscripts.net/raw/Universal-Script-RTetris-76191","テトリス") end})
ScriptHubTab:AddButton({Name = "クロスケ作 v式飛行", Callback = function() executeScript("https://rawscripts.net/raw/Universal-Script-VFly-gui-and-noclip-78112","v式飛行") end})

-- 情報とサポートタブ
local InfoTab = Window:MakeTab({Name = "情報とサポート", Icon = "rbxassetid://4483362458", PremiumOnly = false})
InfoTab:AddSection({Name = "バージョン情報"})
InfoTab:AddLabel("Tsukuyomihub やおよろー！")
InfoTab:AddLabel("さくらhub v0.6 統合")
OrionLib:Init()
