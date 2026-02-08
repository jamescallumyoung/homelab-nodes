objects = ada0.ign ada1.ign ada2.ign ada3.ign ada4.ign

# the "cluster master" is the first server node to be set up.
# other nodes connect to this one during cluster creation.
# note: make sure the "Node role assignments" below match the selection here.
cluster_master = ada0.lan

# k3s configuration types
k3s_args_master = INSTALL_K3S_EXEC=\"server\"
k3s_args_server = K3S_URL=\"https://$(cluster_master):6443\" INSTALL_K3S_EXEC=\"server\"
k3s_args_agent 	= K3S_URL=\"https://$(cluster_master):6443\" INSTALL_K3S_EXEC=\"agent\"

# Node role assignments
ada0_k3s_args = $(k3s_args_master)
ada1_k3s_args = $(k3s_args_server)
# all other nodes default to agent

# calculate each node's hostname and k3s exec args
hostname  = $(shell basename $@ .tbu)
k3s_args  = $(or $($(hostname)_k3s_args),$(k3s_args_agent))

all: $(objects)

%.ign: %.tbu
	butane $< -p -o $@

%.tbu: ada.bu
	cp $< $@
	sed -i -e "s/%HOST_NAME%/$(hostname)/g" \
	       -e "s|%K3S_INSTALL_ARGS%|$(k3s_args)|g" \
	       $@

.PHONY: clean
clean:
	rm -rf $(objects)
