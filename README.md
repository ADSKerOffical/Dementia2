Привет, Денис, и те, кто зашёл в этот репозиторий. Я сделал этот репозиторий на случай, если при каких‑либо обстоятельствах я забуду, как делать скрипты — все учебные материалы я оставил здесь. Я также хочу сделать сильные скрипты, которые могут натворить беды из ничего. 
Вот мои пожелания и те скрипты, которые я сделал:

# Anti part fling:
  Описание: отключает коллизии с тех объектов, которые не принадлежат локальному игроку
  Сделано: ✓
  Исходный код:
  ```lua
getgenv().isnetworkowner = newcclosure(function(part)
  return (part.ReceiveAge == 0 and gethiddenproperty(part, "NetworkIsSleeping") == false)
end)

game.Players.LocalPlayer.SimulationRadius = math.huge
game.Players.LocalPlayer.SimulationRadiusChanged:Connect(function()
  game.Players.LocalPlayer.ReplicationFocus = workspace
  sethiddenproperty(game.Players.LocalPlayer, "SimulationRadius", math.huge)
  sethiddenproperty(game.Players.LocalPlayer, "MaxSimulationRadius", math.huge)
  sethiddenproperty(game.Players.LocalPlayer, "MaximumSimulationRadius", math.huge)
end)

game:GetService("RunService").PreSimulation:Connect(function()
local parts = workspace:GetPartBoundsInRadius(game.Players.LocalPlayer.Character.HumanoidRootPart.Position, 50)
  for _, part in next, parts do
    if part.Anchored == false and not isnetworkowner(part) and part.Velocity.Magnitude >= 4 and not game.Players:GetPlayerFromCharacter(part.Parent) then
part.CanCollide = isnetworkowner(part)
part.CanTouch = isnetworkowner(part)
part.Velocity = Vector3.zero
part.CustomPhysicalProperties = PhysicalProperties.new(0, 0, 0)
part.Massless = not isnetworkowner(part)
part.AssemblyLinearVelocity = Vector3.zero
part.AssemblyAngularVelocity = Vector3.zero

 for _, phys in next, part:GetDescendants() do
   if phys:IsA("Constraint") or phys:IsA("BodyMover") then
phys:Destroy()
   end
 end
 
    end
  end
 end)
  ```
  
# Touch TouchTransmitter to all players:
  Описание: прикосает все ближайшие неприкреплённые объекты ко всем игрокам
  Сделано: ✓
  Исходный код:
  ```lua
  while task.wait() do
 if game.Workspace:FindFirstChild(game.Players.LocalPlayer.Name) and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
  game.Players.LocalPlayer.SimulationRadius = math.huge
  for _, part in next, workspace:GetPartBoundsInRadius(game.Players.LocalPlayer.Character.HumanoidRootPart.Position, 40) do
    if part:FindFirstChild("TouchInterest") then
      for _, player in next, game.Players:GetPlayers() do
         if player ~= game.Players.LocalPlayer and workspace:FindFirstChild(player.Name) then
           for _, parte in next, player.Character:GetChildren() do
             if parte:IsA("BasePart") then
               task.spawn(function()
                 firetouchinterest(part, parte, 0)
                 firetouchinterest(part, parte, 1)
               end)
             end
           end
        end
      end
    end
  end
 end
end
  ```
# Anti network ownership control:
  Описание: заставляет все ближайшие контролируемые читером объекты подчиниться тебе
  Сделано: ×

# Anti anti fling:
  Описание: когда игрок прыгает, то его HipHeight хитбокс на миллисекунду игнорирует локальные изменения коллизии (для других игроков). Можно ли этим самым способом сделать Anti anti fling?
  Сделано: ×

# Get module script from require asset:
  Описание: получает модульный скрипт из require rbxassetid со стороны клиента
  Сделано: ×

# Invisible fling:
  Описание: при изменении Model.HumanoidRootPart.Position хитбокс персонажа перемещается, но сама модель остаётся на месте. Возможно ли из этого сделать невидимый флинг?
  Сделано: ×

# Change LocalPlayer Value for RemoteEvents:
  Описание: изменяет сетевые пакеты и заставляет думать сервер, что данный RemoteEvent запустил другой игрок. В теории это возможно так как из клиента получается информация о том кто запустил этот RemoteEvent
  Сделано: ×

# Anti chat admin:
  Описание: единственный простой способ контролировать всех тех, кто запустил скрипт создателя скриптов это чат. Данный скрипт отключает все Connections с чата. Сейчас чат админ практически никто не использует
  Сделано: ✓
  Исходный код:
  ```lua
  if game.TextChatService.ChatVersion == Enum.ChatVersion.LegacyChatService then
 game:GetService("RunService").Heartbeat:Connect(function()
   for _, player in next, game.Players:GetPlayers() do
     for _, con in next, getconnections(player.Chatted) do
       con:Disconnect()
     end
   end
 end)
elseif game.TextChatService.ChatVersion == Enum.ChatVersion.TextChatService then
  game:GetService("RunService").Heartbeat:Connect(function()
    for _, con in next, getconnections(game.TextChatService.MessageReceived) do
      con:Disconnect()
    end
  end)
end
```

# Animations crash script
