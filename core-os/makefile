objects = ada0.ign ada1.ign ada2.ign ada3.ign ada4.ign

ip_base   = 192.168.22
ipv6_base = 2a02:168:48d7

cluster_cidr = 10.42.0.0\/16,$(ipv6_base):ad42::\/64
service_cidr = 10.43.0.0\/16,$(ipv6_base):ad43::\/112

# k3s configuration types
k3s_args_master = INSTALL_K3S_EXEC=\"server\"
k3s_args_server = K3S_URL=\"https://ada0.lan:6443\" INSTALL_K3S_EXEC=\"server\"
k3s_args_agent 	= K3S_URL=\"https://ada0.lan:6443\" INSTALL_K3S_EXEC=\"agent\"

# Node role assignments
ada0_k3s_args = $(k3s_args_master)
ada1_k3s_args = $(k3s_args_server)
# all other nodes default to agent

# calculate each node's values (hostname and ip)
# note: the last digit of the ip is the last character of the hostname, so the hostname MUST end in a unique digit
hostname  = $(shell basename $@ .tbu)
suffix    = $(shell expr $(shell echo $(hostname) | rev | cut -c1) + 10)
ipv4      = $(ip_base).$(suffix)
ipv6      = $(ipv6_base):0::$(suffix)
k3s_args  = $(or $($(hostname)_k3s_args),$(k3s_args_agent))

all: $(objects)

%.ign: %.tbu
	butane $< -p -o $@

%.tbu: ada.bu
	cp $< $@
	sed -i -e "s/%HOST_NAME%/$(hostname)/g" \
	       -e "s/%NODE_IPV4%/$(ipv4)/g" \
	       -e "s/%NODE_IPV6%/$(ipv6)/g" \
	       -e "s/%CLUSTER_CIDR%/$(cluster_cidr)/g" \
	       -e "s/%SERVICE_CIDR%/$(service_cidr)/g" \
	       -e "s|%K3S_INSTALL_ARGS%|$(k3s_args)|g" \
	       $@

.PHONY: clean
clean:
	rm -rf $(objects)
