# Data-Structures
This program contains Routes as graph, refuel requests queued, route corrections stored in stack, vehicle type hierarchy in tree, refuel stations in linked list, and hash map for location lookup.
#include <iostream>
#include <map>
#include <vector>
#include <queue>
#include <stack>
#include <list>
#include <unordered_map>
using namespace std;
class Graph {
public:
map<string, vector<string>> routes;
void addRoute(string src, string dest) {
routes[src].push_back(dest);
routes[dest].push_back(src);
}
void showRoutes() {
cout << "\n--- Routes ---\n";
for (auto &r : routes) {
cout << r.first << " -> ";
for (auto &n : r.second) cout << n << " ";
cout << endl;
}
}
};
class RefuelQueue {
public:
queue<pair<string,string>> q;
void requestRefuel(string v, string s) { q.push({v,s}); }
void processRefuel() {
if (!q.empty()) {
auto x = q.front(); q.pop();
cout << "Refueled " << x.first << " at " << x.second << endl;
} else cout << "No refuel requests.\n";
}
};
class RouteStack {
public:
stack<string> st;
void pushCorrection(string c) { st.push(c); }
void undoCorrection() {
if (!st.empty()) {
cout << "Undo: " << st.top() << endl;
st.pop();
} else cout << "No corrections.\n";
}
};
class VehicleNode {
public:
string name;
vector<VehicleNode*> children;
VehicleNode(string n) { name = n; }
void addChild(VehicleNode* c) { children.push_back(c); }
void display(int level=0) {
for(int i=0;i<level;i++) cout<<" ";
cout << "- " << name << endl;
for(auto c:children) c->display(level+1);
}
};
class StationList {
public:
list<string> stations;
void addStation(string s) { stations.push_back(s); }
void showStations() {
cout << "\n--- Refuel Stations ---\n";
for (auto &s : stations) cout << s << endl;
}
};
class LocationMap {
public:
unordered_map<string,string> loc;
void addLocation(string c, string co) { loc[c] = co; }
void getLocation(string c) {
if (loc.find(c)!=loc.end()) cout << "Location of " << c << ": " << loc[c] << endl;
else cout << "Location not found.\n";
}
};
int main() {
Graph g;
g.addRoute("A","B");
g.addRoute("B","C");
g.addRoute("A","D");
g.showRoutes();
RefuelQueue rq;
rq.requestRefuel("Truck1","StationA");
rq.requestRefuel("Car2","StationB");
rq.processRefuel();
rq.processRefuel();
RouteStack rs;
rs.pushCorrection("Rerouted A->B via D");
rs.pushCorrection("Changed speed limit B->C");
rs.undoCorrection();
cout << "\n--- Vehicle Hierarchy ---\n";
VehicleNode* root = new VehicleNode("Vehicle");
VehicleNode* car = new VehicleNode("Car");
VehicleNode* truck = new VehicleNode("Truck");
car->addChild(new VehicleNode("Electric Car"));
car->addChild(new VehicleNode("Diesel Car"));
truck->addChild(new VehicleNode("Mini Truck"));
truck->addChild(new VehicleNode("Heavy Truck"));
root->addChild(car);
root->addChild(truck);
root->display();
StationList sl;
sl.addStation("StationA");
sl.addStation("StationB");
sl.addStation("StationC");
sl.showStations();
LocationMap lm;
lm.addLocation("A","(10.1,20.3)");
lm.addLocation("B","(11.4,21.8)");
lm.getLocation("B");
return 0;
}
