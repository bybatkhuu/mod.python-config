# onion_config

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/bybatkhuu/module.python-config/2.build-publish.yml?logo=GitHub)](https://github.com/bybatkhuu/module.python-config/actions/workflows/2.build-publish.yml)
[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/bybatkhuu/module.python-config?logo=GitHub)](https://github.com/bybatkhuu/module.python-config/releases)
[![PyPI](https://img.shields.io/pypi/v/onion-config?logo=PyPi)](https://pypi.org/project/onion-config)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/onion-config?logo=Python)](https://docs.conda.io/en/latest/miniconda.html)

`onion_config` is a python package that allows for easy configuration management. It allows for loading and validating configuration data from environment variables and config files in JSON and YAML formats.

`Pydantic` based custom config package for python projects.

## Features

- **Main config** based on **Pydantic schema** - <https://pypi.org/project/pydantic>
- Load **environment variables** - <https://pypi.org/project/python-dotenv>
- Load from **multiple** configs directories
- Load configs from **YAML** and **JSON** files
- Update the default config with additional configurations (**`extra_dir`** directory)
- **Pre-load hook** function to modify config data before loading and validation
- **Validate config** values with **Pydantic validators**
- Config as **dictionary** or **Pydantic model** (with type hints)
- **Pre-defined** base config schema for common config (**`BaseConfig`**)
- **Base** for custom config loader (**`ConfigLoader`**)

---

## Installation

### 1. Prerequisites

- **Python (>= v3.8)**
- **PyPi (>= v23)**

### 2. Install onion-config package

> [!WARNING]
> If you wanted to use **Pydantic-v1**, but if you already installed `pydantic-settings` and `pydantic-core`, remove it before installing **Pydantic-v1**:

```sh
pip uninstall -y pydantic-settings
pip uninstall -y pydantic-core

# Then install with Pydantic-v1:
pip install -U onion-config[pydantic-v1]
```

> [!WARNING]
> If you wanted to use **Pydantic-v2**, but if you already installed `onion-config` package just by '**`pip install -U onion-config`**' command, and this didn't install `pydantic-settings`. \
> For this case, '**`env_prefix`**' **WILL NOT WORK** for `BaseConfig` or `BaseSettings` without `pydantic-settings`! This is Pydantic-v2's problem, and there could be some other problems. \
> So fix these issues re-install `onion-config` with `pydantic-settings`:

```sh
pip install -U onion-config[pydantic-settings]
```

Choose one of the following methods to install the package **[A ~ F]**:

**A.** [**RECOMMENDED**] Install from **PyPi**

```sh
# Install or upgrade package:
# Pydantic-v1:
pip install -U onion-config[pydantic-v1]

# Pydantic-v2:
pip install -U onion-config[pydantic-settings]
```

**B.** Install latest version from **GitHub**

```sh
# Install package by git:
# Pydantic-v1:
pip install git+https://github.com/bybatkhuu/module.python-config.git[pydantic-v1]

# Pydantic-v2:
pip install git+https://github.com/bybatkhuu/module.python-config.git[pydantic-settings]
```

**C.** Install from **pre-built release** files

1. Download **`.whl`** or **`.tar.gz`** file from **releases** - <https://github.com/bybatkhuu/module.python-config/releases>
2. Install with pip:

```sh
# Pydantic-v1:
# Install from .whl file:
pip install ./onion_config-[VERSION]-py3-none-any.whl[pydantic-v1]
# Or install from .tar.gz file:
pip install ./onion_config-[VERSION].tar.gz[pydantic-v1]

# Pydantic-v2:
# Install from .whl file:
pip install ./onion_config-[VERSION]-py3-none-any.whl[pydantic-settings]
# Or install from .tar.gz file:
pip install ./onion_config-[VERSION].tar.gz[pydantic-settings]
```

**D.** Install from **source code** by building package

```sh
# Clone repository by git:
git clone https://github.com/bybatkhuu/module.python-config.git onion_config
cd ./onion_config

# Install python build tool:
pip install -U pip build

# Build python package:
python -m build

_VERSION=$(./scripts/get-version.sh)

# Pydantic-v1:
# Install from .whl file:
pip install ./dist/onion_config-${_VERSION}-py3-none-any.whl[pydantic-v1]
# Or install from .tar.gz file:
pip install ./dist/onion_config-${_VERSION}.tar.gz[pydantic-v1]

# Pydantic-v2:
# Install from .whl file:
pip install ./dist/onion_config-${_VERSION}-py3-none-any.whl[pydantic-settings]
# Or install from .tar.gz file:
pip install ./dist/onion_config-${_VERSION}.tar.gz[pydantic-settings]
```

**E.** Install with pip editable **development mode** (from source code)

```sh
# Clone repository by git:
git clone https://github.com/bybatkhuu/module.python-config.git onion_config
cd ./onion_config

# Install with editable development mode:
# Pydantic-v1:
pip install -e .[pydantic-v1]

# Pydantic-v2:
pip install -e .[pydantic-settings]
```

**F.** Manually add to **PYTHONPATH** (not recommended)

```sh
# Clone repository by git:
git clone https://github.com/bybatkhuu/module.python-config.git onion_config
cd ./onion_config

# Install python dependencies:
# Pydantic-v1:
pip install -r ./requirements.txt

# Pydantic-v2:
pip install -r ./requirements.pydantic-v2.txt

# Add current path to PYTHONPATH:
export PYTHONPATH="${PWD}:${PYTHONPATH}"
```

## Usage/Examples

To use `onion_config`, import the `ConfigLoader` class from the package:

```python
from onion_config import ConfigLoader, BaseConfig
```

You can create an instance of `ConfigLoader` with `auto_load` flag. This will automatically load configuration data from environment variables and config files located in the default directory (`'./configs'`). The configuration data can then be accessed via the `config` property of the `ConfigLoader` instance:

```python
config: BaseConfig = ConfigLoader(auto_load=True).config
```

### **Simple**

[**`.env`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/simple/.env)

```sh
ENV=production
```

[**`configs/1.base.yml`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/simple/configs/1.base.yml):

```yaml
env: test

app:
  name: "My App"
  version: "0.0.1"
  nested:
    key: "value"
```

[**`configs/2.extra.yml`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/simple/configs/2.extra.yml):

```yaml
app:
  name: "New App"
  nested:
    some: "value"
  description: "Description of my app."

another_val:
  extra: 1
```

[**`main.py`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/simple/main.py)

```python
import pprint

from loguru import logger
try:
    import pydantic_settings

    _has_pydantic_settings = True
except ImportError:
    _has_pydantic_settings = False

from onion_config import ConfigLoader, BaseConfig


class ConfigSchema(BaseConfig):
    env: str = "local"

try:
    config: ConfigSchema = ConfigLoader(config_schema=ConfigSchema).load()
except Exception:
    logger.exception("Failed to load config:")
    exit(2)

if __name__ == "__main__":
    logger.info(f"All: {config}")
    logger.info(f"App name: {config.app['name']}")

    if _has_pydantic_settings:
        # Pydantic-v2:
        logger.info(f"Config:\n{pprint.pformat(config.model_dump())}\n")
    else:
        # Pydantic-v1:
        logger.info(f"Config:\n{pprint.pformat(config.dict())}\n")
```

Run the [**`examples/simple`**](https://github.com/bybatkhuu/module.python-config/tree/main/examples/simple):

```sh
cd ./examples/simple

python ./main.py
```

**Output**:

```txt
2023-09-01 00:00:00.000 | INFO     | __main__:<module>:29 - All: env='production' another_val={'extra': 1} app={'name': 'New App', 'version': '0.0.1', 'nested': {'key': 'value', 'some': 'value'}, 'description': 'Description of my app.'}
2023-09-01 00:00:00.000 | INFO     | __main__:<module>:30 - App name: New App
2023-09-01 00:00:00.000 | INFO     | __main__:<module>:35 - Config:
{'another_val': {'extra': 1},
 'app': {'description': 'Description of my app.',
         'name': 'New App',
         'nested': {'key': 'value', 'some': 'value'},
         'version': '0.0.1'},
 'env': 'production'}
```

### **Advanced**

[**`.env.base`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/advanced/.env.base):

```sh
ENV=development
DEBUG=true
APP_NAME="Old App"
ONION_CONFIG_EXTRA_DIR="extra_configs"
```

[**`.env.prod`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/advanced/.env.prod):

```sh
ENV=production
APP_NAME="New App"
APP_SECRET="my_secret"
```

[**`configs/config.yml`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/advanced/configs/config.yml):

```yaml
env: local

app:
  name: "My App"
  port: 9000
  bind_host: "0.0.0.0"
  version: "0.0.1"
  ignore_val: "Ignore me"

logger:
  output: "file"
```

[**`configs/logger.json`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/advanced/configs/logger.json):

```json
{
    "logger": {
        "level": "info",
        "output": "stdout"
    }
}
```

[**`configs_2/config.yml`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/advanced/configs_2/config.yml):

```yaml
extra:
  config:
    key1: 1
```

[**`configs_2/config_2.yml`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/advanced/configs_2/config_2.yml):

```yaml
extra:
  config:
    key2: 2
```

[**`extra_configs/extra.json`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/advanced/extra_configs/extra.json):

```json
{
    "extra": {
        "type": "json"
    }
}
```

[**`schema.py`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/advanced/schema.py):

```python
from enum import Enum
from typing import Union

import pydantic
from pydantic import Field, SecretStr
_has_pydantic_settings = False
if "2.0.0" <= pydantic.__version__:
    try:
        from pydantic_settings import SettingsConfigDict

        _has_pydantic_settings = True
    except ImportError:
        pass

from onion_config import BaseConfig


# Environments as Enum:
class EnvEnum(str, Enum):
    LOCAL = "local"
    DEVELOPMENT = "development"
    TEST = "test"
    DEMO = "demo"
    STAGING = "staging"
    PRODUCTION = "production"

# App config schema:
class AppConfig(BaseConfig):
    name: str = Field("App", min_length=2, max_length=32)
    bind_host: str = Field("localhost", min_length=2, max_length=128)
    port: int = Field(8000, ge=80, lt=65536)
    secret: SecretStr = Field(..., min_length=8, max_length=64)
    version: str = Field(..., min_length=5, max_length=16)
    description: Union[str, None] = Field(None, min_length=4, max_length=64)

    if _has_pydantic_settings:
        # Pydantic-v2:
        model_config = SettingsConfigDict(extra="ignore", env_prefix="APP_")
    else:
        # Pydantic-v1:
        class Config:
            extra = "ignore"
            env_prefix = "APP_"

# Main config schema:
class ConfigSchema(BaseConfig):
    env: EnvEnum = Field(EnvEnum.LOCAL)
    debug: bool = Field(False)
    app: AppConfig = Field(...)
```

[**`config.py`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/advanced/config.py):

```python
from loguru import logger

from onion_config import ConfigLoader

from schema import ConfigSchema


# Pre-load function to modify config data before loading and validation:
def _pre_load_hook(config_data: dict) -> dict:
    config_data["app"]["port"] = "80"
    config_data["extra_val"] = "Something extra!"
    return config_data

config = None
try:
    _config_loader = ConfigLoader(
        config_schema=ConfigSchema,
        configs_dirs=["configs", "configs_2", "/not_exists/path/configs_3"],
        env_file_paths=[".env", ".env.base", ".env.prod"],
        pre_load_hook=_pre_load_hook,
        config_data={"base": "start_value"},
        warn_mode="ALWAYS",
    )
    # Main config object:
    config: ConfigSchema = _config_loader.load()
except Exception:
    logger.exception("Failed to load config:")
    exit(2)

```

[**`main.py`**](https://github.com/bybatkhuu/module.python-config/blob/main/examples/advanced/main.py):

```python
import pprint

from loguru import logger
try:
    import pydantic_settings
    _has_pydantic_settings = True
except ImportError:
    _has_pydantic_settings = False

from config import config


if __name__ == "__main__":
    logger.info(f"All: {config}")
    logger.info(f"ENV: {config.env}")
    logger.info(f"DEBUG: {config.debug}")
    logger.info(f"Extra: {config.extra_val}")
    logger.info(f"Logger: {config.logger}")
    logger.info(f"App: {config.app}")
    logger.info(f"Secret: '{config.app.secret.get_secret_value()}'\n")

    if _has_pydantic_settings:
        # Pydantic-v2:
        logger.info(f"Config:\n{pprint.pformat(config.model_dump())}\n")
    else:
        # Pydantic-v1:
        logger.info(f"Config:\n{pprint.pformat(config.dict())}\n")

    try:
        # This will raise ValidationError
        config.app.port = 8443
    except Exception as e:
        logger.error(f"{e}\n")
```

Run the [**`examples/advanced`**](https://github.com/bybatkhuu/module.python-config/tree/main/examples/advanced):

```sh
cd ./examples/advanced

python ./main.py
```

**Output**:

```txt
2023-09-01 00:00:00.000 | INFO     | onion_config._base:load:143 - Loading all configs...
2023-09-01 00:00:00.000 | WARNING  | onion_config._base:_load_dotenv_file:201 - '/home/user/workspaces/projects/onion_config/examples/advanced/.env' file is not exist!
2023-09-01 00:00:00.000 | WARNING  | onion_config._base:_load_configs_dir:257 - '/not_exists/path/configs_3' directory is not exist!
2023-09-01 00:00:00.000 | SUCCESS  | onion_config._base:load:171 - Successfully loaded all configs!
2023-09-01 00:00:00.000 | INFO     | __main__:<module>:19 - All: env=<EnvEnum.PRODUCTION: 'production'> debug=True app=AppConfig(name='New App', bind_host='0.0.0.0', port=80, secret=SecretStr('**********'), version='0.0.1', description=None) extra={'config': {'key1': 1, 'key2': 2}, 'type': 'json'} extra_val='Something extra!' logger={'output': 'stdout', 'level': 'info'} base='start_value'
2023-09-01 00:00:00.000 | INFO     | __main__:<module>:20 - ENV: production
2023-09-01 00:00:00.000 | INFO     | __main__:<module>:21 - DEBUG: True
2023-09-01 00:00:00.000 | INFO     | __main__:<module>:22 - Extra: Something extra!
2023-09-01 00:00:00.000 | INFO     | __main__:<module>:23 - Logger: {'output': 'stdout', 'level': 'info'}
2023-09-01 00:00:00.000 | INFO     | __main__:<module>:24 - App: name='New App' bind_host='0.0.0.0' port=80 secret=SecretStr('**********') version='0.0.1' description=None
2023-09-01 00:00:00.000 | INFO     | __main__:<module>:25 - Secret: 'my_secret'

2023-09-01 00:00:00.000 | INFO     | __main__:<module>:30 - Config:
{'app': {'bind_host': '0.0.0.0',
         'description': None,
         'name': 'New App',
         'port': 80,
         'secret': SecretStr('**********'),
         'version': '0.0.1'},
 'base': 'start_value',
 'debug': True,
 'env': <EnvEnum.PRODUCTION: 'production'>,
 'extra': {'config': {'key1': 1, 'key2': 2}, 'type': 'json'},
 'extra_val': 'Something extra!',
 'logger': {'level': 'info', 'output': 'stdout'}}

2023-09-01 00:00:00.000 | ERROR    | __main__:<module>:36 - "AppConfig" is immutable and does not support item assignment
```

---

## Running Tests

To run tests, run the following command:

```sh
# Pydantic-v1:
pip install -r ./requirements.txt

# Pydantic-v2:
pip install -r ./requirements.pydantic-v2.txt

# Install python test dependencies:
pip install -r ./requirements.test.txt

# Run tests:
python -m pytest -v
```

## FAQ

### What is the order of loading config?

Load order:

1. Load all dotenv files from `env_file_paths` into environment variables.
2. Check if required environment variables exist or not.
3. Load all config files from `configs_dirs` into `config_data`.
4. Load extra config files from `extra_dir` into `config_data`.
5. Execute `pre_load_hook` method to modify `config_data`.
6. Init `config_schema` with `config_data` into final **`config`**.

## Environment Variables

You can use the following environment variables inside [**`.env.example`**](https://github.com/bybatkhuu/module.python-config/blob/main/.env.example) file:

```sh
ONION_CONFIG_EXTRA_DIR="./extra_configs"
```

## Documentation

- [docs](https://github.com/bybatkhuu/module.python-config/blob/main/docs/README.md)
- [scripts](https://github.com/bybatkhuu/module.python-config/blob/main/docs/scripts/README.md)

---

## References

- <https://docs.pydantic.dev>
- <https://github.com/pydantic/pydantic>
- <https://docs.pydantic.dev/latest/usage/pydantic_settings>
- <https://github.com/pydantic/pydantic-settings>
- <https://saurabh-kumar.com/python-dotenv>
- <https://github.com/theskumar/python-dotenv>
- <https://packaging.python.org/tutorials/packaging-projects>
