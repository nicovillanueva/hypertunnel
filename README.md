# Tunneler

## Configuration
By default, tunneler searches for a `config.yml` file next to it. You can set the path to a different one by the `-c` parameter.

The configuration file is made up of 2 (well, 3) sections: `tunnels` and `hops` (and `logging`)

### Tunnels/Port forwards
The tunnels are defined in the manner `<local_port>:<remote_host>:<remote_port>` much like in the SSH's command -L parameter or the SSH config file.

The remote host refers to whose `remote_port` will be exposed locally as `local_port`.

Say you configure three hops: `host1`, `host2` and `host3`; and on the tunneling, you configure something like `8081:secureHost1:8080`, then you'll be able to connect to `localhost:8081` which will be tunneled through to `secureHost1:8080` after conecting to it through `host3`.

Kinda like: `your_machine` -> `host1` -> `host2` -> `host3` -> `secureHost1`

### Hops
Hosts that will make the "chain" described above, in order.

The required (and only) parameters are `host`, `user` and `auth`. `auth` itself can have two possible options: `key` and `password`.  
The `key` should be a relative or absolute path to the authentication RSA key. Environment variables are not supported, but `~` expansion is.

## Installing
Requirements are listed in `requirements.txt`. As always, recommended to use virtualenvs.

### Docker
Also it's possible to run this with Docker. However, you may have to mount the `config.yml` with volumes. Same with the SSH key. And not to mention that you need to give it access to your network.

#### Example
After `docker build -t tunneler .`,

    docker run -ti -v $HOME/.ssh/mykey:/root/.ssh/mykey:ro \
               -v $(pwd)/src/config.yml:/data/config.yml:ro \
               --rm --net=host tunneler


## TODO:
- Randomize intermediate ports
- Clean up created ports if shit went down
