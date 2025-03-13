# **💳 bininfos**  
**_Real-time BIN database for fetching and uploading. This is an ongoing project._**  

---

## 🚀 **Example Code: JavaScript**
```javascript
async function checkBin(binNumber) {
    if (!binNumber || binNumber.length < 6 || !/^\d+$/.test(binNumber)) {
        return "❌ Invalid BIN. Must be at least 6 digits.";
    }

    binNumber = binNumber.slice(0, 6); // Trim to first 6 digits

    try {
        const response = await fetch('https://hannie.me/getbindata/', {
            method: 'POST',
            headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
            body: new URLSearchParams({ ccb: binNumber })
        });

        if (!response.ok) throw new Error('Server error');

        const data = await response.json();

        const info = [
            data.type || 'N/A',
            data.level || 'N/A',
            data.brand ? data.brand.split(' ')[0] : 'N/A'
        ].filter(Boolean).join(' - ');

        const result = `
            **BIN Lookup**\n
            **BIN:** ${binNumber}\n
            **Info:** ${info.toUpperCase()}\n
            **Issuer:** ${data.bank?.toUpperCase() || 'N/A'}\n
            **Country:** ${data.country?.toUpperCase() || 'N/A'}
        `;

        return result;

    } catch (error) {
        return `❌ Error: ${error.message}`;
    }
}

// Example usage
checkBin('420155').then(console.log);
```
***🐘 Example Code: PHP***
```
function checkBin($binNumber) {
    if (!$binNumber || strlen($binNumber) < 6 || !ctype_digit($binNumber)) {
        return "❌ Invalid BIN. Must be at least 6 digits.";
    }

    $binNumber = substr($binNumber, 0, 6); // Trim to first 6 digits

    $url = 'https://hannie.me/getbindata/';
    $postData = http_build_query(['ccb' => $binNumber]);

    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $postData);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, ['Content-Type: application/x-www-form-urlencoded']);

    $response = curl_exec($ch);
    curl_close($ch);

    if (!$response) return "❌ Error: No response from server";

    $data = json_decode($response, true);

    if (!$data) return "❌ Invalid response from server";

    $info = array_filter([
        $data['type'] ?? 'N/A',
        $data['level'] ?? 'N/A',
        isset($data['brand']) ? explode(' ', $data['brand'])[0] : 'N/A'
    ]);
    $infoLine = implode(' - ', $info);

    $result = "
        **BIN Lookup**\n\n
        **BIN:** {$binNumber}\n
        **Info:** " . strtoupper($infoLine) . "\n
        **Issuer:** " . strtoupper($data['bank'] ?? 'N/A') . "\n
        **Country:** " . strtoupper($data['country'] ?? 'N/A') . "
    ";

    return $result;
}

// Example usage
echo checkBin('420155');
```

***🐍 Example Code: Python***
```
import aiohttp
import json

async def check_bin(bin_number):
    if not bin_number.isdigit() or len(bin_number) < 6:
        return "❌ Invalid BIN. Must be at least 6 digits."

    bin_number = bin_number[:6]  # Trim to first 6 digits

    try:
        async with aiohttp.ClientSession() as session:
            async with session.post('https://hannie.me/getbindata/', data={'ccb': bin_number}) as response:
                if response.status != 200:
                    return "❌ Error: Failed to fetch data from server"
                
                data = await response.json()

                info = ' - '.join(filter(None, [
                    data.get('type', 'N/A'),
                    data.get('level', 'N/A'),
                    data.get('brand', '').split(' ')[0] if data.get('brand') else 'N/A'
                ]))

                result = (
                    f"**BIN Lookup**\n"
                    f"**BIN:** {bin_number}\n"
                    f"**Info:** {info.upper()}\n"
                    f"**Issuer:** {data.get('bank', 'N/A').upper()}\n"
                    f"**Country:** {data.get('country', 'N/A').upper()}"
                )

                return result

    except Exception as e:
        return f"❌ Error: {str(e)}"

# Example usage (if using asyncio)
import asyncio
print(asyncio.run(check_bin('420155')))
```
