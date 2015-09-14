# RMTCP - Redundant scheduler for Multitpath TCP (MPTCP)

## Background of MPTCP

Multipath TCP (MPTCP) is an extension to TCP in order to use simultaneously multiple paths. The development of this protocol in the Linux kernel is performed in http://github.com/multipath-tcp/mptcp.

## About the redundant scheduler for MPTCP

This redundant scheduler is based on the default scheduler of MPTCP. The default scheduler selects the best subflow to send a data chunk on the basis of the state (active or backup) and Time to live (TTL) of the subflows. For every data chunk, the active subflow with the best TTL at that moment is selected. So, the data chunk is only sent through the best subflow and the other subflows are only used if retransmissions for that data chunk are required.

On the other hand, this redundant scheduler sends the data replicated through all the active subflows available. The data is not replicated through backup subflows, which are only used in case of requiring retransmissions.

The different behaviour for active and backup subflows provided by the redundant scheduler allows to easily move from a totally replicated traffic policy (all subflows in active mode) to the same traffic policy of the default scheduler (all subflows in backup mode) and even a mix-mode (some subflows active and other ones in backup mode).

This redundant scheduler could be of interest in environments with low throughput but high delay and jitter constrains.

## Acknowledgments

This code was done thanks to the [SECRET project](http://www.secret-project.eu) funded by the European Commission, whose aim is to improve the security of the railway infrastructure against EM attacks.

## References

The general architecture of the [SECRET project](http://www.secret-project.eu), in which the redundant scheduler was implemented and used, is detailed here:

```bibtex
@InProceedings{,
  Title                    = {Cyber attacks in the guided_transport domain},
  Author                   = {Christophe Gransart and Christian Pinedo and Marina Aguado and Marc Heddebaut and Eduardo Jacob and Igor Lopez and Marivi Higuero},
  Booktitle                = {C\&ESAR 2014 - Computer \& Electronics Security Applications Rendez-vous},
  Year                     = {2014},
  Month                    = {nov},
}
```

If you plan to use this scheduler in a publication, use the following reference which explains the behaviour of the scheduler and proposes to use it in SCADA systems.

```bibtex
@InProceedings{,
  Title                    = {SCADA systems in the railway domain: Enhancing reliability through Redundant MultipathTCP},
  Author                   = {Igor Lopez and Marina Aguado and Christian Pinedo and Eduardo Jacob},
  Booktitle                = {ITSC 2015},
  Year                     = {2015},
  Month                    = {sep},
}
```

## Installation

### From deb package

We have prepared deb packages to easily test/use the redundant scheduler in Ubuntu AMD64 (and probably Debian AMD64). The scheduler is compiled inside the kernel and it is configured as the default scheduler for MPTCP.

The deb packages are available in this [server](http://files.i2t.ehu.eus/rmptcp).

### From source code

Just compile this Linux kernel as the [usual way](README). Previously to the compilation, ensure that you have selected the redundant scheduler in the MPTCP configuration section.

## Usage

1. Verify hat you are using the redundant scheduler for MPTCP.

```bash
~# sysctl net.mptcp.mptcp_scheduler  
net.mptcp.mptcp_scheduler = redundant
```

2. If not, configure MPTCP with the redundant scheduler. Alternatively, you can configure /etc/sysctl.conf to use this scheduler on every reboot.

```bash
~# sysctl net.mptcp.mptcp_scheduler=redundant  
net.mptcp.mptcp_scheduler = redundant
```
