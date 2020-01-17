# TEST2

- controller 생성 

  - ip 변경 

    - vi /etc/sysconfig/network-scripts/ifcfg-ens33 

    ```
  IPADDR="10.0.0.200"   
    ```

    - systemctl restart network 

  - hosts 변경 

    - vi /etc/hosts 
  
    ```
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
    10.0.0.200 controller
    10.0.0.201 compute1
    
    ```
  
  

  - compute1 생성
  
    - ip 변경 
  
      - vi /etc/sysconfig/network-scripts/ifcfg-ens33 
  
      ```
      IPADDR="10.0.0.201"   
      ```
  
      - systemctl restart network 
  
    - hosts 변경
  
      - vi /etc/hosts 
  
      ```
      127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
      ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
      10.0.0.200 controller
      10.0.0.201 compute1
      
      ```
  
  
  
  
  - Openstack repository(rdo) 등록 
  
    ```shell
    $ yum install –y centos-release-openstack-rocky
  $ yum repolist
    $ yum upgrade -y   // vm power off 
  $ yum install -y openstack-packstack*   //Pcckstack설치
    $ packstack --gen-answer-file=/root/openstack.txt
    $ cp /root/openstack.txt /root/openstack.orig   //파일 카피 
    $ vi /root/openstack.txt 
     #CONFIG_DEFAULT_PASSWORD=abc123 
     #CONFIG_CEILOMETER_INSTALL=n 
     #CONFIG_AODH_INSTALL=n
     #CONFIG_KEYSTONE_ADMIN_PW=abc123 
     #CONFIG_HEAT_INSTALL=y
     #CONFIG_MAGNUM_INSTALL=y
     #CONFIG_TROVE_INSTALL=y
     #CONFIG_NEUTRON_L2_AGENT=openvswitch CONFIG_NEUTRON_OVS_BRIDGE_IFACES=br-ex:ens33
     #COMPUTE_COMPUTE_HOSTS=10.0.0.200,10.0.0.201
    $ time packstack --answer-file=/root/openstack.txtglance의 backend stores를 swift와 연결
    
    ```
  
  
  
  
  
  ### glance의 backend stores를 swift와 연결 
  
  - 설정 파일 수정
    -  $ vi /etc/glance/glance-api.conf
  
  ```shell
    #### 주의 #####
    # 혹시 모르니  os_region_name=RegionOne   수정 후=>   os_region_name = RegionOne
    # 죄다 사이 간격을 띄워주자
    
    #[glance_store]
    stores = file,http,swift
    default_store = swift
    filesystem_store_detadir = /var/lib/glance/images/
  
    #cat .keystone_admin 에서 address 확인하고 수정해주기 뇨
    swift_store_auth_version = 2  
    # 2 => 3
    swift_store_auth_address = http://controller:5000/v2.0/  
    # => http://10.0.0.200:5000/v3
    
    
    swift_store_user = service:swift 
    # => services:swift
    
    
    swift_store_key = swift_password
    # => 루트에 있는 openstack.txt 파일에서 CONFIG_SWIFT_KS_PW 비번 확인하고 변경
    
    swift_store_create_container_on_put = True
    
    swift_store_container = glance
    os_region_name = RegionOne
    
    
    
  ```
  
    
  
  - 설정 파일 수정 후에 glance 서비스들을 다시 시작하고 포트번호를 확인
  
    ```shell
    - systemctl restart openstack-glance-api openstack-glance-registry
    ```
  



- http://10.0.0.200/dashboard/project/images 가서 관리자로 로그인

1. compute->이미지로 가서 cirros이미지를 생성 (cirros 이미지가 가벼워서 빨리 생성돼서 그냥 쓴듯?)
   - 포멧은 QCOW2

2. 생성했으면 관리자-ID 말고 SWIFT-ID 로 로그인해서 오브젝트 스토리지.컨테이너에 이미지가 올라왔는지 확인        ->    올라왔으면 성공