### Virtual env activation
```bash
python -m venv myenv
source myenv/bin/activate
deactivate
```

### Conda
- альтернатива, умеет скачивать не только python пакеты
- скачивает последовательно

```
conda create -n <env_name>
conda activate <env_name>
conda deactivate
```

### Mamba 
- шустрее и параллельное скачивание

### Poetry
- еще одна альтернатива для venv and pip
- швейцарско армейский нож
