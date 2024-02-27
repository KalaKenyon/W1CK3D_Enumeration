# W1CK3D_Enumeration

This Python script is a subdomain enumerator that queries DNS for each combination of words from a specified wordlist appended to a given domain.

## Usage

1. Clone the repository or download the script file.
2. Make sure you have Python installed on your system.
3. Install the required dependencies by running the following command:

   ```bash
   pip install aiohttp
   ```

4. Prepare your wordlist file containing a list of words, with each word on a separate line.
5. Modify the script to specify the target domain and the path to your wordlist file.
6. Run the script using the command:

   ```bash
   dns_subdomain_enumerator.py
   ```

## Features

- Asynchronously queries DNS for subdomains using aiohttp.
- Rate limits requests to prevent overwhelming the server.
- Handles connection errors and reports them to the user.

## Example

```python
import asyncio
import aiohttp

async def query_dns(session, url):
    try:
        async with session.get(url) as response:
            if response.status == 200:
                print(f"Subdomain found: {url}")
    except aiohttp.ClientError as e:
        print(f"Error querying DNS for {url}: {e}")

async def main(wordlist, domain):
    async with aiohttp.ClientSession() as session:
        tasks = []
        for word in wordlist:
            url = f"http://{domain}/{word}"
            tasks.append(query_dns(session, url))
            # Rate limiting: Sleep for a short duration between queries
            await asyncio.sleep(0.1)  # Adjust sleep duration as needed

        await asyncio.gather(*tasks)

if __name__ == "__main__":
    domain = "WEBSITEHERE.COM"
    wordlist = []
    with open("WORDLISTFILEHERE.txt", "r") as file:
        for line in file:
            wordlist.append(line.strip())

    asyncio.run(main(wordlist, domain))
```

## License

No license.

## Contributing

Contributions are welcome! If you find a bug or have a suggestion for improvement, please open an issue or submit a pull request.

## Disclaimer

This tool should be used responsibly and with proper authorization. The author is not responsible for any misuse or damage caused by this software.

