objects = ada0.ign ada1.ign ada2.ign ada3.ign ada4.ign

# the "cluster master" is the first server node to be set up.
# other nodes connect to this one during cluster creation.
# note: make sure the "Node role assignments" below match the selection here.
cluster_master = ada0.lan

# Node role assignments
ada0_k3s_role = master
ada1_k3s_role = server
# all other nodes default to agent

# calculate each node's hostname and k3s role
hostname  = $(shell basename $@ .tbu)
role  = $(or $($(hostname)_k3s_role),agent)

all: $(objects)

%.ign: %.tbu
	butane $< -p -o $@

%.tbu: ada.bu
	cp $< $@
	sed -i -e "s/%HOST_NAME%/$(hostname)/g" \
	       -e "s/%CLUSTER_MASTER_ADDR%/$(cluster_master)/g" \
	       -e "s/%ROLE%/$(role)/g" \
	       $@

.PHONY: clean
clean:
	rm -rf $(objects)
