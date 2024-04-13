# Implementing Round Robin Algorithm
struct Process {
    int id;
    int arrivalTime;
    int burstTime;
    int remainingTime;
};

void roundRobinScheduling(std::vector<Process>& processes, int quantum) {
    queue<Process> readyQueue;
    int currentTime = 0;
    int totalProcesses = processes.size();

    for  (int i = 0; i < totalProcesses; i++) {
        processes[i].remainingTime = processes[i].burstTime;
    }

    while (!readyQueue.empty() || currentTime < totalProcesses) {
        for (int i = currentTime; i < totalProcesses; i++) {
            if (processes[i].arrivalTime <= currentTime) {
                readyQueue.push(processes[i]);
                currentTime++;
            } else {
                break;
            }
        }

        if (!readyQueue.empty()) {
            Process currentProcess = readyQueue.front();
            readyQueue.pop();

            int executionTime = std::min(quantum, currentProcess.remainingTime);
            currentProcess.remainingTime -= executionTime;

            std::cout << "Executing process ID: " << currentProcess.id << " for time: " << executionTime << std::endl;

            if (currentProcess.remainingTime > 0) {
                readyQueue.push(currentProcess);
            }
        } else {
            currentTime++;
        }
    }
}
