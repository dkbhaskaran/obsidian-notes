Introduction
--

This entry is mostly from the perspective of learning the basic concepts involved in today's buzz words Artificial intelligence and machine learning. In here we cover some key landmarks in evolution of AI/ML which led to the modern view of ChatGPT or similar applications also known as Large language models by navigating through a little bit of peer review papers, math and programs using PyTorch. The goal is not to approach the subject theoretically rather intuitively using basic math and simple programs.

Artificial intelligence as the name suggests is defined by ability of machines to perform activities like logical reasoning, problem-solving, decision making. This is pretty wide term encompassing machine learning, deep leaning and natural language processing etc.

A hierarchical representation of the terminologies that are important to our discussion is as below

Artificial Intelligence (AI)
│
├── Symbolic AI / GOFAI – Rule-based systems using logic and symbols
│   ├── Expert Systems – Emulate decisions of human experts
│   ├── Rule-Based Systems – Use "if-then" rules for decision making
│   └── Knowledge Representation & Reasoning – Represent facts and infer new ones
│
├── Machine Learning (ML) – Learning patterns from data
│   ├── Supervised Learning – Learn from labeled data
│   │   ├── Decision Trees – Tree-like decision paths
│   │   ├── SVM – Classify data using optimal boundaries
│   │   └── Neural Networks – Brain-inspired layered models
│   │       └── Deep Neural Networks (DNN) – Multi-layered neural networks
│   │           ├── CNN – Image and spatial data processing
│   │           ├── RNN – Sequence and time-series processing
│   │           └── Transformers – Self-attention based models
│   │               └── Generative AI – Create new content (text, image, etc.)
│   │                   └── LLMs – Large language models trained on text (e.g. ChatGPT)
│   │
│   ├── Unsupervised Learning – Find hidden structure in data
│   │   ├── Clustering – Group similar data points (e.g., K-means)
│   │   └── Dimensionality Reduction – Compress features while keeping structure (e.g., PCA)
│   │
│   └── Reinforcement Learning – Learn by trial and error
│       ├── Q-Learning – Value-based decision learning
│       └── DQN – Combines deep learning with Q-learning
│
└── Other Subfields of AI
    ├── NLP – Understand and generate human language
    ├── Computer Vision – Interpret visual input like images/videos
    ├── Robotics – Perceive and act in the physical world
    └── Planning & Scheduling – Determine action sequences to achieve goals


It should be understood that the AI is not just ChatGPT or vice versa.  LLMs have become by far the most popular applications on internet today as their numerous applications are being invented/discovered daily. The whole world is evolving rapidly by the application of LLMs. To build an good understanding of how LLMs work, one should start from understanding DNNs at the very least.

Deep neural networks (DNN)
---

To understand the working of a LLM like ChatGPT it is helpful to understand the basics of DNNs and how they work. There will be few notable details or terms that we will cover here, which will be used again in the discussion of LLMs. A basic building block for a DNN is a mathematical model called perceptron. These were introduced as part of a 1958 paper "[The Perceptron: A Probabilistic Model for Information Storage and Organization in the Brain]( https://www.ling.upenn.edu/courses/cogs501/Rosenblatt1958.pdf)" by Frank Rosenblatt pioneering the field of AI.

In this paper F. Rosenblatt discusses a mathematical model called perceptron, analogous to human neuron, that can process information, learn and adapt (based on feedback). This is similar to a function that takes in several independent inputs and produces an output. The output is calculated based on the weighted sum of the inputs passed through a activation function. Much like the diagram below

![[Pasted image 20250506103038.png]]

