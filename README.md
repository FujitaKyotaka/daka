# Daka

Check the holidays, and your personal events, then daka!!

## Usage

Use `Crontab`, `GitHub Actions`, or `Docker` to run the schedule.

### Crontab

- Install

```bash
npm install
```

- Copy `example.env` to `.env`
- Change the required env variables
  - MODULE: the daka module
  - USERNAME: your username
  - PASSWORD: your password
- Edit the `crontab`

```bash
crontab -e

0 10 * * * /NODE_PATH/node /DAKA_FOLDER/src/index.js S >>/LOG_FOLDER/daka.log 2>&1
0 19 * * * /NODE_PATH/node /DAKA_FOLDER/src/index.js E >>/LOG_FOLDER/daka.log 2>&1
```

### GitHub Actions

> [Note](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule): The schedule event may be delayed.

- Modify the schedule you want (GitHub Actions use UTC time)
- After each daka, GitHub Actions open an issue and then close it immediately. (So an email will be sent to inform the status of daka)

```yaml
on:
  schedule:
    - cron: '30 0 * * *'
    - cron: '0 11 * * *'
```

- Add required secrets to GitHub Actions
  - MODULE: the daka module
  - USERNAME: your username
  - PASSWORD: your password

### Docker

- Pull the image from Docker Hub or GitHub packages or build your own docker image

```bash
docker pull jyunhanlin/daka:latest

# or

docker pull ghcr.io/jyunhanlin/daka:latest

# or build your own image

docker build -t daka .
```

- Run the image with username and password

```bash
docker run -e MODULE=module -e USERNAME=your_username -e PASSWORD=your_password DAKA_IMAGE
```

### Other environment variables (optional)

| env variable     | default | description                                                                   |
| ---------------- | :-----: | ----------------------------------------------------------------------------- |
| DELAY_START_MINS |    5    | the delay mins before start daka, range from 0 to DELAY_START_MINS            |
| DELAY_END_MINS   |   15    | the delay mins before end daka, range from DELAY_START_MINS to DELAY_END_MINS |
| IMMEDIATE_DAKA   |  false  | immediate daka without delay                                                  |
| MAX_RETRY_COUNT  |    3    | total retry times                                                             |
