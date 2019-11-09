# Switch_mac_address_table
routing_table = {}  # will store switch mac_address table
class interface():
  def __init__ (self,name):
    self.interface_Name = "fastEthernet 0/" + name
  def set_ipAddress(self,ip):
    self.ipAddress = ip
  def set_ip_Mask (self,mask):
    self.ip_Mask = mask
  def set_macAddress(self,mac_Address):
    self.macAddress = mac_Address
  def set_Speed (self,speed):
    self.speed = speed
  def set_Duplex(self,duplex):
    self.duplex = duplex
  def set_interface_cost(self,cost):
    self.cost = cost
  def set_Vlan (self,vlan):
    self.vlan = vlan 
  def set_State (self,state):
    self.state = state
  def get_ipAddress (self):
    return self.ipAddress
  def get_ip_Mask (self):
    return self.ip_Mask
  def get_macAddress(self):
    return self.macAddress
  def get_Speed(self):
    return self.speed
  def get_Duplex(self):
    return self.duplex
  def get_Vlan (self):
    return self.vlan
  def get_State (self):
    return self.state
  def get_Cost (self):
    return self.cost
  def get_int_name(self):
    return self.interface_Name
class host():
  def __init__(self,name,ip,mac):
    self.ipAddress = ip
    self.macAddress = mac
    self.name = name
  def set_name (self,name):
    self.name = name
  def set_ipAddress (self,ip):
    self.ipAddress = ip
  def set_macAddress (self,mac):
    self.macAddress = mac
  def get_ipAddress (self):
    return self.ipAddress
  def get_macAddress (self):
    return self.macAddress
  def get_name (self):
    return self.name
class packet():
  def __init__ (self,source_macAddress,dest_macAddress,source_ipAddress,dest_ipAddress):
    self.source_macAddress = source_macAddress
    self.dest_macAddress = dest_macAddress
    self.source_ipAddress = source_ipAddress
    self.dest_ipAddress = dest_ipAddress
  def set_src_mac (self,src_mac):
    self.source_macAddress = src_mac
  def set_dst_mac (self,dst_mac):
    self.dest_macAddress = dst_mac
  def set_src_ip (self,src_ip):
    self.source_ipAddress = src_ip
  def set_dst_ip (self,dst_ip):
    self.dest_ipAddress = dst_ip
  def get_src_mac (self):
    return self.source_macAddress
  def get_dst_mac (self):
    return self.dest_macAddress
  def get_src_ip (self):
    return self.source_ipAddress
  def get_dst_ip (self):
    return self.dest_ipAddress
    
def connnection(Host):
  Connection = {Host_1:Fa_one,Host_2:Fa_two, Host_3:Fa_three, Host_4:Fa_four}
  return Connection[Host]
  
def switch_routing(Packet,interface):
  routing_table[Packet.get_src_mac()] = interface.get_int_name()
  
#This switch has four interface

Fa_one = interface(str(1))
Fa_one.set_macAddress("aaaa.aaaa.aaaa")
Fa_one.set_ipAddress("192.168.1.1")

Fa_two = interface(str(2))
Fa_two.set_macAddress("bbbb.bbbb.cccc")
Fa_two.set_ipAddress("192.168.1.3")

Fa_three = interface(str(3))
Fa_three.set_macAddress("cccc.cccc.cccc")
Fa_three.set_ipAddress("192.168.1.5")

Fa_four = interface(str(4))
Fa_four.set_macAddress("dddd.dddd.dddd")
Fa_four.set_ipAddress("192.168.1.7")
#Interaces.append(Fa_one,Fa_two,Fa_three,Fa_four)
Hosts = [] 
  #Host Mike,Rose,Nat,Bill
Host_1 = host("Mike","192.168.1.2","0002.414c.8b01") #connected to int 1
Hosts.append(Host_1)
Host_2 = host("Rose","192.168.1.4","0004.414c.8b01") #connected to int 2
Hosts.append(Host_2)
Host_3 = host("Nat","192.168.1.6","0006.414c.8b01")  #connected to int 3
Hosts.append(Host_3)
Host_4 = host("Bill","192.168.1.8","0008.414c.8b01") #connected to int 4
Hosts.append(Host_4)
hosts = ['null']
hosts.append(Host_1)
hosts.append(Host_2)
hosts.append(Host_3)
hosts.append(Host_4)




#creating a packet from Host 1 to Host 3
while True:
  print("1: Send a packet   2:Show mac-address table")
  options = int(input("What would you like to do? "))
  if options == 1:
    user_src = int(input("Enter source host (1-4): "))
    user_dst = int(input("Enter destination host (1-4): "))
    interface = connnection(hosts[user_src])
    Packet = packet(hosts[user_src].get_macAddress(),interface.get_macAddress(),hosts[user_src].get_ipAddress(),hosts[user_dst].get_ipAddress())
    #add the packet to the switch routing table if the mac doesn't already exits
    if hosts[user_src].get_macAddress() not in routing_table:
      switch_routing(Packet,interface)
    if hosts[user_dst].get_macAddress() not in routing_table:
      print("Unknown unicast: Packet sent out to all interfaces")
    else:
      print("Packet will be forwarded out throught {}".format(routing_table[hosts[user_dst].get_macAddress()]))
  if options == 2:
    print("----------------------------")
    print("Host |  mac_address       |   Interface")
    print("----------------------------")
    for x in routing_table:
      print("{}   |   {}".format(x,routing_table[x]))
