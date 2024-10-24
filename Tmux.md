# Jupiter notebook starter in TMUX windows

## How to start?
First, install system requirements:
- python
- python-venv
- tmux

```bash
./install.sh
```

## How to use?

1. to start 3 jupiter environments in tmux windows use:

```bash
./main.sh start 3
```

2. To stop all tmux windows use:

```bash
./main.sh stop_all
```

3. To stop one tmux window by id use command:

```bash
./main.sh stop id
```

#### Note:

- all tmux window installs and creates new python venv (it will take some time to install packages);
- each venv has installed jupyter-lab;
- you can connect to jupyter-lab using localhost:attached_port
- use `tmux ls` to watch activated jupyter-lab windows


#### 0. install.sh
```bash
#!/bin/bash

sudo apt update
sudo apt install -y python3 python3-venv tmux

echo " ==== Finished ==== "
```

#### 1. main.sh
```
#!/bin/bash

SESSION_NAME="jupyter_session"
INIT_PORT=10001   # start from
BASE_DIR="./dir"

BYellow='\033[1;33m'      # Yellow
BBlue='\033[1;34m'        # Blue
BPurple='\033[1;35m'      # Purple
BCyan='\033[1;36m'        # Cyan
NC='\033[0m' # No Color

start() {
    N=$1
    # start tmux session
    tmux new-session -d -s "${SESSION_NAME}"

    for i in $(seq 0 $(($N-1))); do
        DIR="${BASE_DIR}${i}"
        mkdir -p "$DIR"
        
        # make some python stuff
        python3 -m venv "$DIR/venv"
        source "$DIR/venv/bin/activate"
        pip install -q --upgrade pip  # quiet mode
        pip install -q jupyter
        
        # set base port and rnd token
        PORT=$(($INIT_PORT + $i))
        TOKEN=$(openssl rand -hex 12)

        # start tmux window
        tmux new-window -t "${SESSION_NAME}:${i}"
        tmux send-keys -t "${SESSION_NAME}:${i}" "source $DIR/venv/bin/activate && jupyter notebook --ip=0.0.0.0 --port=$PORT --no-browser --NotebookApp.token='$TOKEN' --NotebookApp.notebook_dir='$DIR'" C-m

	      echo "========================================================================"
        echo -e "${BPurple}==== Created new TMUX window id: $i, port: $PORT, jupyter-lab token: $TOKEN${NC}"
	      echo "========================================================================"
    done
  echo -e "${BPurple}$(tmux list-windows)${NC}"
}

# kill session by id
stop() {
    i=$1
    tmux kill-window -t "${i}"
    rm -rf "${BASE_DIR}${i}"
    echo -e "${BYellow}===== Session window stopped by: $i ===========${NC}"
    echo -e "${BYellow} $(tmux list-windows) ${NC}"
}


stop_all() {
    tmux kill-session -t "${SESSION_NAME}"
    echo -e "${BCyan}===== All session windows: ${SESSION_NAME} were killed =========${NC}"
    echo -e "${BCyan} $(tmux list-windows) ${NC}"
    rm -rf "${BASE_DIR}*"
    echo -e "${BCyan}===== All dirs removed!${NC}"
}

# user set args handler
case $1 in
    start)
        start $2
        ;;
    stop)
        stop $2
        ;;
    stop_all)
        stop_all
        ;;
    *)
        echo "How to use: ./main.sh start N | stop i | stop_all"
        ;;
esac
```
