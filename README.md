# test_graph-updated

#include <iostream>
#include <sstream>
#include <fstream>

#include "graph.h"
#include "graph_algorithms.h"
#include "timer.h"

using namespace std;

int main() {
  timer t;
  t.start();

  // Create a graph.
  graph<int, double> g;
  graph<int, double> football;
  g.insert_vertex(5);
  g.insert_vertex(4);
  g.insert_edge(0, 1, 0.5);
  //g.insert_edge(1, 0, 0.25);
  g.insert_vertex(6);
  g.insert_vertex(9);
  g.insert_vertex(10);
  g.insert_edge(2, 1, 0.25);
  g.insert_edge(3, 1, 0.1);
  g.insert_edge(1, 4, 3);
  g.insert_edge(2, 4, .3);
  g.insert_edge(3, 4, 100);
  
  t.stop();
  cout << "Creating the graph took " << t.elapsed() / 1e6 << " ms" << endl;
  t.restart();

  // Exercise iterators and accessors.
  for(auto vi = g.vertices_begin(); vi != g.vertices_end(); ++vi)
    cout << vi->second->descriptor() << " " << vi->second->property() << endl;

  for(auto vi = g.vertices_cbegin(); vi != g.vertices_cend(); ++vi)
    cout << vi->second->descriptor() << " " << vi->second->property() << endl;

  for(auto ei = g.edges_begin(); ei != g.edges_end(); ++ei)
    cout << ei->second->source() << " " << ei->second->target() << " "
         << ei->second->property() << endl;

  for(auto ei = g.edges_cbegin(); ei != g.edges_cend(); ++ei)
    cout << ei->second->source() << " " << ei->second->target() << " "
         << ei->second->property() << endl;

  // Exercise find
  graph<int, double>::edge_descriptor ed(0, 1);
  
  auto iterator = g.find_edge(ed);
   if (iterator ==g.edges_end())
	   cout << "Error edge doesn't exist";
  cout << (g.find_vertex(0))->second->property() << " "
       << (g.find_edge(ed))->second->property()
       << endl;

ifstream footballgraph {"football.g"};
  footballgraph >> football;
  cout << football;

  std::map<int, int> pm;
  breadth_first_search(g, 0, pm);
  auto trav = pm.begin();
  int first;
  int second;
  while(trav != pm.end()) {
    first = trav->first;
    second = trav->second;
    std::cout<<first<<" things "<<second<<std::endl;
    trav++;
  }
  
  std::map<int, int> p;
  mst_prim_jarniks(g, p);
  auto trav1 = p.begin();
  int first1;
  int second1;
  while(trav1 != p.end()) {
    first1 = trav1->first;
    second1 = trav1->second;
    std::cout<<first1<<" things2 "<<second1<<std::endl;
    trav1++;
  }

  // Exercise erase
  g.erase_edge(ed);
  g.erase_vertex(0);

  // Exercise output
  cout << g;

  t.stop();
  cout << "Exercises took " << t.elapsed() / 1e6 << " ms" << endl;
}
