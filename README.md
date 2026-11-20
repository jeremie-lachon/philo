# philo

A multithreaded simulation of the dining philosophers problem, written in C with POSIX threads and mutexes.

Philosophers sit around a table with one fork between each pair of neighbours. Each philosopher needs both adjacent forks to eat, then sleeps, then thinks, and dies if too long passes between meals. The goal is to keep everyone alive without deadlocks or data races.

## How it works

- Each philosopher is a thread; each fork is a mutex.
- Threads wait until all philosophers are ready, then start at the same time.
- Even-numbered philosophers start slightly later, so neighbours don't all grab their left fork at once and deadlock.
- A monitor thread checks each philosopher's last meal time (protected by its own mutex) and stops the simulation as soon as one dies, or once everyone has eaten enough.
- All output goes through a shared mutex, so log lines never interleave and nothing is printed after a death.

## Build

```bash
make
```

## Usage

```bash
./philo <philosophers> <time_to_die> <time_to_eat> <time_to_sleep> [meals_required]
```

Times are in milliseconds. If `meals_required` is given, the simulation ends once every philosopher has eaten that many times.

```bash
./philo 4 410 200 200 3
```

```
0ms 1 has taken a fork
0ms 1 has taken a fork
0ms 1 is eating
...
All philos have eaten 3 times.
```
