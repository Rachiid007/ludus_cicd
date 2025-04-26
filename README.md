# ludus CI/CD 🚀

This repo uses a GitHub Actions pipeline to deploy the Ludus Range config. Here's a breakdown of what the workflow does:

1. **Checkout Repository** 🛠️  
   Uses [actions/checkout@v4](https://github.com/actions/checkout) to clone the repo.

2. **Validate & Format YAML** 📄  
   - Confirms that `config/ludus/ludus-range-config.yml` exists.
   - Removes trailing whitespace.
   - Formats the YAML file using [yq](https://github.com/mikefarah/yq) and re-validates with `yamllint`.
   - Inserts blank lines between list items for clarity.

3. **Setup Environment** ⚙️  
   Installs necessary packages like WireGuard, `jq`, and `expect`.

4. **WireGuard VPN Configuration** 🔐  
   - Decodes the base64-encoded WireGuard configuration from the secret `WIREGUARD_CONFIG` into `wg0.conf` file.
   - Activates the VPN with `wg-quick up`.

5. **Ludus CLI Installation & Verification** 📦

6. **Ludus API Key Validation** 🔑  
   Ensures that the `LUDUS_API_KEY` secret is set and available, which is critical for authenticating Ludus CLI commands.

7. **Deployment Process** 🚀  
   - Applies the configuration and initiates a deployment with `ludus range config set` and `ludus range deploy`.
   - Monitors the deployment status and displays logs upon completion or failure.

8. **Cleanup** 🧹  
   Always disconnects the WireGuard VPN after deployment, ensuring the environment is reset.

## Environment Variables & Secrets

Make sure the following GitHub secrets are configured in your repository:

- **LUDUS_API_KEY**  
  Your Ludus API key.  

  Example:
  ```sh
  gh secret set LUDUS_API_KEY -b "xxx" --repo <your_repo_name>
  ```

- **WIREGUARD_CONFIG**  
  Base64 encoded content of your WireGuard configuration file (e.g., .conf file) for easier processing in the pipeline.

  Example:
  ```sh
  gh secret set WIREGUARD_CONFIG -b "xxx" --repo <your_repo_name>
  ```

Enjoy deploying with ease! 🎉
