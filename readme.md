*What is this about?*
Netscanjs is intended for educational pruposes only. I am currently working on a blog that uses this netscanjs to illustrate examples. Once the blog has been published I will add a link to it and improve this readme. Netscanjs is limited in many ways, but it is simple to use and worked well for my purposes.

*Copy and paste convenience functions:*
var scan = new netscan({
    verbosity: nsVerbosity.Debug, //defaults to nsVerbosity.None
    waitOnPortMaxInterval: 2200, //defaults to 2000ms. I honestly don't know what a good default is here. What is optimal for subnet and host scanning is not optimal for port scanning.
    maxWebSocketConnectionsToUniqueHosts: 100 //Defaults to 200 which is the default maximum number of allowable websocket connections by firefox.
});

var scanForSubnets = function(firstTwoOctets){
    scan.scanSubnets(firstTwoOctets, {
        discoveredHosts: function(hosts){
            console.log('subNets returned to calling code: ', hosts);
        },
        onErrorHandler: function(error) {
            console.log('Error From calling code: Line '+ error.lineno + ' in ' + error.filename + ': ' + error.message);
        }});
};

var scanSubnetForHosts = function(subnet){
    scan.scanSubnetForHosts(subnet, {
        discoveredHosts: function(hosts){
            console.log('hosts returned to calling code: ', hosts);
        },
        onErrorHandler: function(error) {
            console.log('Error From calling code: Line '+ error.lineno + ' in ' + error.filename + ': ' + error.message);
        }});
};


var scanHostForPorts = function(host, startPort, endPort){
    scan.scanHostForPorts(host, startPort, endPort, {
        discoveredPorts: function(openPorts){
            console.log('ports returned to calling code: ', openPorts);
        },
        onErrorHandler: function(error) {
            console.log('Error From calling code: Line '+ error.lineno + ' in ' + error.filename + ': ' + error.message);
        }});
};

*Exampmles:*

It is important to not call these functions concurrently. If you want to be able to use them together then you will need to make use of the callback functions. I regret not using promises now, but again the point of this library is for illustration and is not meant to be part of a solution.

_To scan for subnets)
scanForSubnets([192,168]);

_To scan a subnet for hosts_
scanSubnetForHosts('192.168.10.1');

_To get the hosts internal IP addresss_
scan.getInternalIpAddress(function(ip){
     console.log('Internal IP = ' + ip);
});

_To get the hosts external IP addresses_
scan.getExternalIpAddress(function(ip){
    console.log('External IP = ' + ip);
 });  