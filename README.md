# SciFlow project using Parsl to execute Scientific Workflows on HPC resources

The majority of tasks that we, as researchers and analysts, perform, are conveniently expressible as scientific workflows. These workflows provide abstraction, integration and reusability, and thereby eases the scientific knowledge discovery process. However, as the complexity of the scientific problem increases, the complexity of the workflow increases proportionately. To facilitate such interactions, workflows utilize complex connectors. 

The SciFlow framework provides a set of compositional channel connectors in a control thread, which can be used to construct a variety of workflow patterns. We separate computational components from coordination components by writing a separate control thread to provide easier workflow modification and component reusability to users. 

SciFlow is a flexible workflow management system which can be executed on a multitude of HPC resources due to the implicit parallelism that Parsl provides. 

## SciFlow Workflow Connectors
A SciFlow workflow is constructed by stringing together individual Python-Parsl computational units, coordinated by a Golang control thread using ‘connectors’. With SciFlow’s channel connectors, the user is able to connect computational components in a variety of commonly used workflow patterns, as suited to his application. 
The diagram below depicts an example scientific workflow, which makes use of three SciFlow connector types. 
1. Sequence Connector for sequential execution of workflow modules
2. Parallel Split Connector for splitting the coordination thread into concurrent modules
3. Exclusive Choice Connector to enforce dynamic decision making. 

<img src="https://github.com/SciFlow-FYP/SciFlow-for-Parsl-blog/blob/main/diagrams/SciFlow-ExampleWorkflow.png" alt="Example SciFlow Workflow" width="800" class="center"/>

SciFlow focuses on dynamic decision making, where the outcome of an intermediary workflow component would be used to determine the path that the remainder of the workflow would take. This can be facilitated by automating the workflow to run a number of times parallely, with different parameters. Connectors can be used to select the best performing parameter set based on some performance matric. Based on this selection the workflow path would be modified at runtime, to yield better quality results.

## Parsl in SciFlow 
The use of Parsl in SciFlow is two fold; to perform data parallelism on individual computational components and to provide execution-location independence for the entire workflow. 

### Execution location independence
The primary objective of SciFlow is to provide a layer of abstraction to scientific users, so that they could better focus their energies on developing innovative solutions to challenging domain problems, with a lower learning curve for HPC. Through the integration of Parsl within SciFlow, our users are able to harness the full potential of cluster resources for their workflow applications, without the inherent MPI programming burden involved. 

With Parsl’s implicit parallelism, we are able to orchestrate the workflow on any HPC resource with minor code changes. During the initial stages of the SciFlow project, we made use of the parallelism provided by threads on a local computer, and later moved to a more powerful Beowulf cluster. This goes to show that Parsl’s execution location independence allows its users to start small and scale-out with minimal effort as the project progresses.

### Data parallelism 
The scientific workflow applications SciFlow caters to, make use of large datasets. Therefore, we use both data and task parallelism to improve computational efficiency. The framework executes computational modules concurrently using the Golang control thread, thereby providing task parallelism. This proves to be extremely helpful in situations similar to hyper parameter optimisation. Using SciFlow and Parsl, the user has the ability to divide the dataset either horizontally or vertically as suited, to achieve data parallelism. 

The diagram below depicts this process, where task parallelism is achieved with Golang and data parallelism with Parsl. 

<img src="https://github.com/SciFlow-FYP/SciFlow-for-Parsl-blog/blob/main/diagrams/SciFlow-DataParallelism.png" alt="Parallelism in SciFlow" width="800" class="center"/>

