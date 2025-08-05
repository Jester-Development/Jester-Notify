===============================
✅ Jester-Notify Export Setup
===============================

Instructies om Jester-Notify te integreren als vervanger van standaard notificaties in:

- ESX
- QBCore
- QBox

=======================
🔵 ESX Integratie
=======================

Open: `es_extended/client/functions.lua`

Vervang de originele `ESX.ShowNotification` functie met de volgende:

```lua
function ESX.ShowNotification(message, type, length)
    if GetResourceState('jester-notify') ~= 'missing' then
        if type == 'error' then
            exports['jester-notify']:Alert("Fout", message, 5000, 'error')
        elseif type == 'inform' then
            exports['jester-notify']:Alert("Info", message, 5000, 'inform')
        elseif type == 'success' then
            exports['jester-notify']:Alert("Succes", message, 5000, 'success')
        elseif type == 'warning' then
            exports['jester-notify']:Alert("Waarschuwing", message, 5000, 'warning')
        else
            exports['jester-notify']:Alert("Info", message, 5000, 'inform')
        end
    else
        print('[jester-notify]: Resource niet gevonden of niet gestart!')
    end
end
```

=======================
🟣 QBCore Integratie
=======================

Open: `qb-core/client/functions.lua`

Vervang de originele `QBCore.Functions.Notify` functie met:

```lua
function QBCore.Functions.Notify(text, textype, length)
    if GetResourceState('jester-notify') ~= 'missing' then
        if textype == 'primary' then textype = 'inform' end 
        local ttype = textype or "inform"
        local length = length or 5000
        exports['jester-notify']:Alert("", text, length, ttype)
    else
        print('[jester-notify]: Resource niet gevonden of niet gestart!')
    end
end
```

=======================
🟡 QBox Integratie
=======================

QBox gebruikt meestal een eigen wrapper of systeem gebaseerd op exports.

Voeg dit toe waar jouw QBox notificatie logic zit (bijv. `client/functions.lua` of custom scripts):

```lua
function Notify(text, type, time)
    local notifyType = type or "inform"
    local notifyTime = time or 5000

    if GetResourceState('jester-notify') ~= 'missing' then
        exports['jester-notify']:Alert("", text, notifyTime, notifyType)
    else
        print('[jester-notify]: Resource niet gevonden of niet gestart!')
    end
end
```

Gebruik dan overal in je scripts:
```lua
Notify("Welkom op de server!", "success", 5000)
```

=======================
🎨 Beschikbare Types
=======================
- error
- inform
- success
- warning
- orange
- purple
- teal
- pink

=======================
🔗 GitHub Repo
=======================
https://github.com/Jester-Development/Jester-Notify
