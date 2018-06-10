# msbuskinmath.com

- Teaching portfolio website on Wordpress
- hosted / operationalized with Lightsail and Bitnami Wordpress

## Installation

1. Install Wordpress somewhere e.g. Flywheel local for local dev, or work directly on server :)
2. In WP root directory, `git init`
3. `git remote add origin git@github.com:zemccartney/msbuskinmath.com.git`
4. `git pull`

### Requirements

- Access to project's Lightsail Account :)
        
## Notes on Maintaining

- Domain is managed through Google Domains
- Nameservers and zone record configured in Lightsail
- SSL cert managed with Let’s Encrypt
    - Required a few Apache config changes, detailed here: https://metablogue.com/enable-lets-encrypt-ssl-aws-lightsail/ 
- Amazon SES for email service



