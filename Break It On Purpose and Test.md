<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">Break it on purpose (fault-finding practice)</strong></br>
<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">Test 1: strict uRPF on iVIOS-3 Gi0/0. Forward traffic arrives from iVIOS-2, but iVIOS-3’s best route back to 192.168.1.0 is via R7, so uRPF drops it.</strong></br>
<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">iVIOS-3(config)# interface GigabitEthernet0/0</strong></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">iVIOS-3(config-if)# ip verify unicast reverse-path</strong></br>
<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">ping 192.168.7.1 source 192.168.1.1 from iVIOS-1 now fails. Check with show ip interface GigabitEthernet0/0 | include verify|drop. Fix it with loose mode: ip verify unicast source reachable-via any.</strong></br>
<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">Test 2: NAT on one lane only (iVIOS-2). Return traffic comes back via R7, so iVIOS-2 never untranslates it.</strong></br>
<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">iVIOS-2(config)# access-list 10 permit 192.168.1.0 0.0.0.255</strong></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">iVIOS-2(config)# ip nat inside source list 10 interface GigabitEthernet0/1 overload</strong></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">iVIOS-2(config)# interface GigabitEthernet0/0</strong></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">iVIOS-2(config-if)# ip nat inside</strong></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">iVIOS-2(config-if)# interface GigabitEthernet0/1</strong></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">iVIOS-2(config-if)# ip nat outside</strong></br>
<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">Ping from iVIOS-1 to 192.168.7.1 and run show ip nat translations on iVIOS-2. The entry exists, but the replies never come back through iVIOS-2. Fix it by moving NAT to iVIOS-1 or iVIOS-3 (where both lanes meet), or by forcing symmetry.</strong></br>
<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">Test 3: stateful ACL on iVIOS-4. Apply an inbound ACL on iVIOS-4 Gi0/1 or iVIOS-5 Gi0/0 that permits only established TCP, then compare behaviour in each direction.</strong></br>
<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">Test 4: second-diamond failure. Shut iVIOS-3 Gi0/2 (the iVIOS-3-iVIOS-4 link). Forward traffic now moves to R6, so both directions use the bottom lane in the second diamond while the first diamond stays asymmetric.</strong></br>
<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">Test 5: make it symmetric. Set iVIOS-5 Gi0/0 cost to 1 and Gi0/1 to 100, and iVIOS-3 Gi0/0 cost to 1 and Gi0/1 to 100, then re-run both traceroutes.</strong></br>
<br></br>
<strong class="___1qwroh2 fl43uef f19n0e5" data-lexical-text="true">I can add a stateful firewall-style ACL on iVIOS-3, or convert the lab to BGP-based asymmetry using local preference and AS-path prepending.</strong></br>
