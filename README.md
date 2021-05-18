# moat-shitcode-list
A list of potentially exploitable vulnerabilities in the Moat TTT Source Code

# possible exploits
- SQL Injection 

YOU LITERALLY HAVE THE FUCKING ABILITY TO USE PREPARED STATEMENTS and yet we have shit like sv_invs_sql.lua

Take this example here:

```
function m_InsertCompTicket(c, cb, cbf)
    local q = MINVENTORY_MYSQL:query("INSERT INTO moat_comps ( time, steamid, admin, link, ic, ec, sc, item, class, talent1, talent2, talent3, talent4, comment, approved ) VALUES ( UNIX_TIMESTAMP(), '" .. MINVENTORY_MYSQL:escape(util.SafeSteamID(c.steamid)) .. "', '" .. MINVENTORY_MYSQL:escape(c.admin) .. "', '" .. MINVENTORY_MYSQL:escape(c.ticket) .. "', '" .. MINVENTORY_MYSQL:escape(c.ic) .. "', '" .. MINVENTORY_MYSQL:escape(c.ec) .. "', '" .. MINVENTORY_MYSQL:escape(c.sc) .. "', '" .. MINVENTORY_MYSQL:escape(c.item) .. "', '" .. MINVENTORY_MYSQL:escape(c.class) .. "', '" .. MINVENTORY_MYSQL:escape(c.talent1) .. "', '" .. MINVENTORY_MYSQL:escape(c.talent2) .. "', '" .. MINVENTORY_MYSQL:escape(c.talent3) .. "', '" .. MINVENTORY_MYSQL:escape(c.talent4) .. "', '" .. MINVENTORY_MYSQL:escape(c.comments) .. "', '0')")

    function q:onSuccess(data)
        cb(data)
    end

    function q:onError(err)
        cbf(err)
    end

    q:start()
end
```

Instead of using prepared statements like INSERT INTO moat_shit VALUES(?) 
Moat's codebase does INSERT INTO moat_shit VALUES("hi")

defeating the purpose of even trying to prepare a statement or delegating it to an internal wrapper - it doesn't matter, it's still vulnerable.

I don't know the extent of moat / mysqloo's escape function but i can guaruntee you unless it's a bona fide commercial grade WAF someone will find a way to sneak a character past it

- Shitty netcode explotation

I gotta find a good example of this but ohh it's in there
