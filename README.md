# Ansible script to deploy OpenMPI, OpenMP, Intel MKL and CUDA.

## Ansible

```bash
ansible-galaxy role install usegalaxy_eu.cuda
ansible-galaxy role install nvidia.nvidia_driver
```

```bash
# Create your own secret files containing 'mpi_user_password' and 'vault_sudo_password'.
$ ansible-vault create secrets.yml
# Modify secrets if required
$ ansible-vault edit secrets.yml
# Run ansible-playbook
$ ansible-playbook playbook.yml --ask-vault-pass
Vault password: 

PLAY [mpi_nodes] ******************************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************************************
ok: [192.168.0.177]
ok: [192.168.0.114]

TASK [Create mpiuser user] ********************************************************************************************************************************************
changed: [192.168.0.177]
changed: [192.168.0.114]

TASK [Ensure mpiuser has sudo privileges] *****************************************************************************************************************************
ok: [192.168.0.177]
ok: [192.168.0.114]

TASK [Ensure SSH directory exists for mpiuser] ************************************************************************************************************************
ok: [192.168.0.114]
ok: [192.168.0.177]

TASK [Generate SSH key for mpiuser if not exists] *********************************************************************************************************************
ok: [192.168.0.177]
ok: [192.168.0.114]

TASK [Install sshpass if not exists] **********************************************************************************************************************************
ok: [192.168.0.114]
ok: [192.168.0.177]

TASK [Distribute SSH public key to other nodes] ***********************************************************************************************************************
skipping: [192.168.0.114] => (item=192.168.0.114) 
skipping: [192.168.0.114] => (item=192.168.0.177) 
skipping: [192.168.0.114]
skipping: [192.168.0.177] => (item=192.168.0.114) 
skipping: [192.168.0.177] => (item=192.168.0.177) 
skipping: [192.168.0.177]

TASK [Create MPI hosts file] ******************************************************************************************************************************************
ok: [192.168.0.177]
ok: [192.168.0.114]

TASK [Install OpenMPI, MPICH, OpenMP and related packages] ************************************************************************************************************
changed: [192.168.0.114]
changed: [192.168.0.177]

TASK [Ensure 'software-properties-common' is installed] ***************************************************************************************************************
ok: [192.168.0.114]
ok: [192.168.0.177]

TASK [Add missing GPG key for the PPA] ********************************************************************************************************************************
ok: [192.168.0.177]
ok: [192.168.0.114]

TASK [Add the graphics-drivers PPA] ***********************************************************************************************************************************
ok: [192.168.0.177]
ok: [192.168.0.114]

TASK [Update apt cache] ***********************************************************************************************************************************************
ok: [192.168.0.114]
ok: [192.168.0.177]

TASK [Ensure build-essential, CMake and CUDA are installed] ***********************************************************************************************************
ok: [192.168.0.114] => (item=build-essential)
ok: [192.168.0.177] => (item=build-essential)
ok: [192.168.0.114] => (item=cmake)
ok: [192.168.0.177] => (item=cmake)
ok: [192.168.0.114] => (item=dkms)
ok: [192.168.0.177] => (item=dkms)
changed: [192.168.0.114] => (item=nvidia-driver-470)
changed: [192.168.0.177] => (item=nvidia-driver-470)
changed: [192.168.0.114] => (item=nvidia-cuda-toolkit)
changed: [192.168.0.114] => (item=nvidia-utils-470)
changed: [192.168.0.177] => (item=nvidia-cuda-toolkit)
changed: [192.168.0.177] => (item=nvidia-utils-470)

PLAY RECAP ************************************************************************************************************************************************************
192.168.0.114              : ok=13   changed=3    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
192.168.0.177              : ok=13   changed=3    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
```

## Examples

### OpenMPI

```bash
$ cd mpi
$ make all
$ mpirun -np 4 ./hello_mpi
Authorization required, but no authorization protocol specified

Authorization required, but no authorization protocol specified

Authorization required, but no authorization protocol specified

Authorization required, but no authorization protocol specified

Authorization required, but no authorization protocol specified

Authorization required, but no authorization protocol specified

Hello world from processor roota-Default-string, rank 0 out of 4 processors
Hello world from processor roota-Default-string, rank 1 out of 4 processors
Hello world from processor roota-Default-string, rank 2 out of 4 processors
Hello world from processor roota-Default-string, rank 3 out of 4 processors
```

### CUDA

```bash
$ cd cuda
$ mkdir -p build && cd build
$ cmake ..
-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/bsmei/mpi-setup/cuda/build
$ make all
[100%] Built target vector_add
$ ./vector_add
[Vector addition of 50000 elements]
Copy input data from the host memory to the CUDA device
CUDA kernel launch with 196 blocks of 256 threads
Copy output data from the CUDA device to the host memory
Test PASSED
Done
```

## References

[Ansible Galaxy - usegalaxy_eu.cuda](https://galaxy.ansible.com/ui/standalone/roles/usegalaxy_eu/cuda/)

[Keras、NVIDIA等の環境構築をAnsibleで自動構築](https://qiita.com/maeda_mikio/items/9fdb64e703dc0b30bd1b)

[ansible-role-nvidia-driver](https://github.com/NVIDIA/ansible-role-nvidia-driver)

[The Linux Cluster](https://thelinuxcluster.com/2023/08/02/installing-cuda-with-ansible-for-rocky-linux-8/)

[How to install 2 CUDA using Docker file and Ansible Task?](https://forums.docker.com/t/how-to-install-2-cuda-using-docker-file-and-ansible-task/113115/1)