HFO-PLUS
===============
We added three new scenes(CatchingPoints,Moving,Chase and Attack) on the origin HFO enviroment.
## Dependencies
 - Boost-system, filesystem
 - Qt4 [Required for SoccerWindow2 visualizer]: To not build the visualizer, add cmake flag `-DBUILD_SOCCERWINDOW=False`

## Python Dependencies
 - Python 2.7 or 3.2+ (tested with 2.7 and 3.5)
 - numpy

## Install
```
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=RelwithDebInfo ..
make -j4
make install
```
Note: There is no need for 'sudo' with 'make install' - the files will be installed below the starting directory.

#### Python Install [required for python interface]
From the main HFO directory: `pip install [--user] .`

## Demos
From the main HFO directory:
```
./example/passing_agents.sh
```

Start a 1v1 game played by Agent2D:
```
./bin/HFO --offense-npcs=1 --defense-npcs=1 --no-sync
```
If you want to use new scene, others are same as HFO:
```
./bin/HFO --catchpoints  --offense-npcs=1 --defense-npcs=1 --no-sync
```

Start an example agent on the empty goal task. This agent will move
forwards slowly. First start the server: `./bin/HFO --offense-agents=1
--no-sync &` and then connect the agent: `./example/hfo_example_agent`

Or do both in a single command:
```
(./bin/HFO --offense-agents=1 --no-sync &) && sleep 1 && ./example/hfo_example_agent
```

## Example Agents

Example agents are provided in the `example` directory. Below are two
minimal examples:

#### C++ Agent
```c++
HFOEnvironment hfo;
hfo.connectToServer(...);
for (int episode=0; episode<5; episode++) {
  status_t status = IN_GAME;
  while (status == IN_GAME) {
    const std::vector<float>& feature_vec = hfo.getState();
    hfo.act(DASH, 20.0, 0.0);
    //or hfo.act(CATCHPOINT);
    status = hfo.step();
  }
  cout << "Episode " << episode << " ended";
}
```

#### Python Agent
```python
hfo = hfo.HFOEnvironment()
hfo.connectServer(...)
for episode in range(5): # replace with xrange(5) for Python 2.X
  status = IN_GAME
  while status == IN_GAME:
    features = hfo.getState()
    hfo.act(DASH, 20.0, 0.0)
    # or hfo.act(CATCHPOINT)
    status = hfo.step()
  print 'Episode', episode, 'ended'
```

## Documentation
The state and action spaces provided by the HFO domain are documented in the [manual](doc/manual.pdf).
