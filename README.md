# Pug Advanced Paintball.
For any questions please contact me here: here https://discord.gg/jYZuWYjfvq.
For any of my other scripts you can purchase here: https://pug-scripts.tebex.io/category/2141020

# Installation
Make sure you have the dependencies installed
Place the sound files provided into InteractSound\client\html\sounds

# MetaData (DO NOT DO THIS IF YOU HAVE ALREADY DONE THIS FOR THE BATTLE ROYALE SCRIPT)
find [ PlayerData.metadata['jailitems'] = PlayerData.metadata['jailitems'] or {} ] in qb-core/server/player and add this right above or below it

PlayerData.metadata['gameitems'] = PlayerData.metadata['gameitems'] or {}

# put this in qb-ambulance/client/main.lua
RegisterNetEvent('hospital:client:ReviveLittle', function()
    local player = PlayerPedId()

    if isDead then
        SetLaststand(false)
        local playerPos = GetEntityCoords(player, true)
        NetworkResurrectLocalPlayer(playerPos, true, true, false)
        isDead = false
        SetEntityInvincible(player, false)
    elseif InLaststand then
        local playerPos = GetEntityCoords(player, true)
        NetworkResurrectLocalPlayer(playerPos, true, true, false)
        isDead = false
        SetEntityInvincible(player, false)
        SetLaststand(false)
    end

    if isInHospitalBed then
        loadAnimDict(inBedDict)
        TaskPlayAnim(player, inBedDict , inBedAnim, 8.0, 1.0, -1, 1, 0, 0, 0, 0 )
        SetEntityInvincible(player, true)
        canLeaveBed = true
    end

    TriggerServerEvent("hospital:server:RestoreWeaponDamage")
    SetEntityMaxHealth(player, 200)
    SetEntityHealth(player, 200)
    ClearPedBloodDamage(player)
    SetPlayerSprint(PlayerId(), true)
    ResetAll2()
    ResetPedMovementClipset(player, 0.0)
    TriggerServerEvent('hud:server:RelieveStress', 100)
    TriggerServerEvent("hospital:server:SetDeathStatus", false)
    TriggerServerEvent("hospital:server:SetLaststandStatus", false)
end)

function ResetAll2()
    isBleeding = 0
    bleedTickTimer = 0
    advanceBleedTimer = 0
    fadeOutTimer = 0
    blackoutTimer = 0
    onDrugs = 0
    wasOnDrugs = false
    onPainKiller = 0
    wasOnPainKillers = false
    injured = {}

    for k, v in pairs(BodyParts) do
        v.isDamaged = false
        v.severity = 0
    end

    TriggerServerEvent('hospital:server:SyncInjuries', {
        limbs = BodyParts,
        isBleeding = tonumber(isBleeding)
    })

    CurrentDamageList = {}
    TriggerServerEvent('hospital:server:SetWeaponDamage', CurrentDamageList)

    ProcessRunStuff(PlayerPedId())
    DoLimbAlert()
    DoBleedAlert()

    TriggerServerEvent('hospital:server:SyncInjuries', {
        limbs = BodyParts,
        isBleeding = tonumber(isBleeding)
    })
end

# DrawText
Make sure to change the drawtext to whatever you want it to be in the open.lua

# use this in your police call script for police to not get shots fired calls when players are in paintball

local inpaintballmatch = false

RegisterNetEvent("Pug:client:InPaintBallMatchWL")
AddEventHandler("Pug:client:InPaintBallMatchWL", function() -- put this in you police calls for not recieving shots fired while playing paintball
	inpaintballmatch = true
end)

RegisterNetEvent("Pug:client:InPaintBallMatchWLFalse")
AddEventHandler("Pug:client:InPaintBallMatchWLFalse", function() -- put this in you police calls for recieving shots fired again after exiting paintball
	inpaintballmatch = false
end)

example of my shot fired call statement

if IsPedShooting(playerPed) and zoneChance('Shooting', 2, currentStreetName) and not refreshPlayerWhitelistedShooting() and not inpaintballmatch then

# IF YOU ARE HAVING AN ISSUE WHERE YOUR GUN GETS PUT AWAY WHEN THE MATCH STARTS AND ARE USING QB-ANTICHEAT FIND THIS LOOP IN QB-ANTICHEAT/CLIENT/MAIN.LUA AND REPLACE IT WITH THIS
local IsInPaintballMatch = false
RegisterNetEvent("Pug:Anticheat:FixRemovedGun", function ()
    IsInPaintballMatch = not IsInPaintballMatch
end)
CreateThread(function()	-- Check if ped has weapon in inventory --
    while true do
        Wait(5000)

        if LocalPlayer.state.isLoggedIn and not IsInPaintballMatch then

            local PlayerPed = PlayerPedId()
            local player = PlayerId()
            local CurrentWeapon = GetSelectedPedWeapon(PlayerPed)
            local WeaponInformation = QBCore.Shared.Weapons[CurrentWeapon]

            if WeaponInformation ~= nil and WeaponInformation["name"] ~= "weapon_unarmed" then
                QBCore.Functions.TriggerCallback('qb-anticheat:server:HasWeaponInInventory', function(HasWeapon)
                    if not HasWeapon then
                        RemoveAllPedWeapons(PlayerPed, false)
                        TriggerServerEvent("qb-log:server:CreateLog", "anticheat", "Weapon removed!", "orange", "** @everyone " ..GetPlayerName(player).. "** had a weapon on them that they did not have in his inventory. QB Anticheat has removed the weapon.")
                    end
                end, WeaponInformation)
            end
        end
    end
end)


# Advanced Paintball. For any questions please contact me here: here https://discord.gg/jYZuWYjfvq.

Advanced Paintball. For any questions please contact me here: here https://discord.gg/jYZuWYjfvq.

This script is using escrow encryption. Config.lua client/open.lua and server.lua are open

PREVIEW HERE: https://youtu.be/iJnzHxUKRv0

This completely configurable script consist of:
● Runs at 0.0 ms resmon

● This script is completely open source other then client.lua. There is a client/open,lua i can always add more to the open.lua upon request

●  0 known scuff. Was tested over and over with 10+ people to plug every issue that was found.

●  18 players max. Configurable.

●  Two teams. Red & blue with clothing to match. Configurable.

●  5 lives per game. Configurable.

●  A wager amount set to minimum $500 and maximum $25,000. Rearward at the end of the game is wager amount * total-players / winning team. Configurable.

●  33 gun options to choose from. Configurable.

●  Custom commentator recorded by my girlfriend. Requires interact sound. (Sounds files will provide)

●  Each team spawns in on the same radio. (qb-radio) required

●  Unlimited sprint. Configurable.

●  Map selection and 11 maps total. (this script uses ipl to load the maps on the client. Newer ish build is required to use these maps as they are apart of a gta 5 online update)

●  The option to spectate players.

Things that still need to be done consist of:

● Game modes

● Multiple games at the same time.

Requirements consist of:
qb-core
qb-menu
qb-input
InteractSound
nh-context for (displaying pictures of maps)(provided)
+set sv_enforceGameBuild 2372 or highe (older game builds may work but this oen for sure works)