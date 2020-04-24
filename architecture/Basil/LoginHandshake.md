---
layout: page
style: herbal3d
title: "Herbal3D: Authentiction/Login Handshake"
---
# Authentication/Login Handshake

This is a detailed description of the handshakes that happen when a Basil
viewer is connecting to a RaguOS enabled OpenSimulator region.
This describes the messages and the specific authentication/authorization
information that is passed back and forth.

```
Entry.html      ------------------->    Login Service
             {UserName, Password, or whatever}

                <-------------------
                  { Login-JWT-Token }


ENTRY DIALOG INVOKES Basil WITH CONNECTION PARAMETERS
    The parameters are passed as a Base64 encoded JSON string as the "c"
    HTML parameter.

Entry.html      ------------------>     Basil.html
            {
                "comm": {
                    "transportURL": InitialServiceConnectionURL,
                    "transport": "WS",
                    "service": "BasilComm"
                },
                "auth": {
                    "SessionKey": StringUniqifyingThisSession,
                    "UserAuth": Login-JWT-Token
                },
                "OpenSimulator": {
                    "Comment": "If logging onto OpenSimulator, some user/region info",
                    "FN": FirstName,
                    "LN": LastName,
                    "aID": AgentId,
                    "WM": RegionWelcomeMessage",
                    "rX": RegionLocationX,
                    "rY": RegionLocationY,
                    "MSU": MapServerURL
                }
            }

BASIL MAKES INITIAL "LOGIN" CONNECTION TO CONTROLLING SPACESERVER

InitialSpaceServer  <-------------------    Basil
            {
                "Op": OpenSessionReq,
                "SessionAuth": Login-JWT-Token,
                "IProps": {
                    "ClientAuth": AuthorizationTokenForTalkingBackToBasil,
                    "SessionKey": StringUniquifyingThisSession
                }
            }
        (The InitialSpaceServer talks to "Login Service" to verify
            Login-JWT-Token)

InitialSpaceServer ---------------->      Basil
            {
                "Op": OpenSessionResp,
                "SessionAuth": AuthorizationTokenForTalkingBackToBasil,
                "IProps": {
                }
            }

CONTROLLING SPACESERVER TELLS BASIL TO CONNECT TO ADDITIONAL SPACESERVERS
    The initial RaguOS implementation has Basil connect to "SpaceServerCC"
    (SpaceServer Command and Control) who requests Basil to connect to
    SpaceServers for the various content layers (static, dynamic, avatars,
    environment, etc).
    Note that each of these connections have their unique authorization tokens
    and SessionKeys.

InitialSpaceServer ---------------->      Basil
            {
                "Op": MakeConnectionReq,
                "SessionAuth": AuthorizationTokenForTalkingBackToBasil,
                "IProps": {
                    "transportURL": SpaceServer-A-ConnectionURL,
                    "transport": "WS",
                    "service": "BasilComm",
                    "ServiceAuth": TokenForInitiallyAccessingSpaceServerA
                }
            }

SpaceServerA        <-------------------    Basil
            {
                "Op": OpenSessionReq,
                "SessionAuth": TokenForInitiallyAccessingSpaceServerA
                "IProps": {
                    "ClientAuth": SS-A-AuthorizationTokenForTalkingBackToBasil,
                    "SessionKey": SS-A-StringUniquifyingThisSession,
                    (more parameters describing Basil features)
                }
            }
SpaceServerA        ---------------->      Basil
            {
                "Op": OpenSessionResp,
                "SessionAuth": SS-A-AuthorizationTokenForTalkingBackToBasil,
                "IProps": {
                    "SessionAuth": TokenForAccessingSpaceServerA,
                    "SessionKey": SS-A-StringUniquifyingThisSession,
                    "ConnectionKey": StringUniqifyingThisConnection,
                    (other parameters describing SpaceServer features)
                }
            }

SPACESERVERS CREATE ITEMS/ABILITIES
```

## Legal Stuff

This document is covered by [Creative Commons Attribution-NonCommercial 4.0 International].


[JWT]: https://jwt.io/
[JWT RFC]: https://tools.ietf.org/html/rfc7519
[RFC7519]: https://tools.ietf.org/html/rfc7519
[RFC3339]: https://tools.ietf.org/html/rfc3339
[OAuth2]: https://oauth.net/2/
[WGS 1984]: http://earth-info.nga.mil/GandG/publications/tr8350.2/tr8350_2.html
[OpenSimulator]: http://opensimulator.org/
[View Service]: http://loc-loc.net/
[Herbal System]: http://herbal3d.org/
[Basil Viewer]: http://basilviewer.org/
[Pesto]: http://misterblue.github.io/pesto/
[Ragu]: http://misterblue.github.io/ragu/
[BSD License]: http://opensource.org/licenses/BSD-3-Clause
[MIT License]: http://opensource.org/licenses/MIT
[Apache License]: http://opensource.org/licenses/Apache-2.0
[Creative Commons Attribution-NonCommercial 4.0 International]: http://creativecommons.org/licenses/by-nc/4.0/

<!-- vim: ts=2 sw=2 et ai
-->
