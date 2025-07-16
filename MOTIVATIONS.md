# The rgb-protocol organization and RGB 0.11.1

Some important updates about the current state of RGB. Many of us have put a lot of time and effort into this project over the years by contributing code, helping with testing and building projects on top of it. We’re deeply invested and want to see it grow into a solid protocol that’s used by many.

Shaping the future of such a complex project is no easy task. Lately more effort has been put into trying to deliver a codebase that’s ready for real projects to build upon. In the past few months coordinating choices has become harder, which isn't ideal when trying to help make things better. Different ways to work through these challenges and find a common path forward have been tried, leading to the parallel development of two concurrent versions.

Version 0.11.1, has been started with the goal of fixing existing issues, removing unused parts, simplifying when possible and increasing test coverage of core components and real-world scenarios. Version 0.12 is a substantial rewrite of 0.11, like 0.11 was a significant rewrite of 0.10 and so did its predecessors, with ambitious goals and different priorities.

A decision to join efforts and focus on the completion of version 0.11.1 has been made but unfortunately collaboration turned out to be harder than expected and we felt the project needed it to thrive, for the benefit of everyone involved.  

So, after a lot of thought and discussion, a new organization called rgb-protocol has been created in order to provide access to the completed work on version 0.11.1. This is an attempt at completing the roadmap everyone has been working on for a long time, focusing on a working version of RGB that is simpler, well tested, understood by a larger portion of the developer community and well documented.

The main goal is to deliver a base reliable implementation and then to build upon and continue to improve it. The emphasis is on stability, simplicity, solid testing and clear documentation. Other projects like rgb-lib already use it and strive to make adoption smooth. Higher level projects like rgb-lightning-node, which uses rgb-lib internally, are being actively maintained to cover further use cases like Lightning Network support.

Everyone is welcome to join the community, where everyone should feel welcome to contribute, share ideas and help shape the project's future.

Different project versions can sometimes lead to fragmentation, which is definitely not the aim of rgb-protocol. Having different approaches can ultimately benefit the entire community, giving people more options that fit their specific needs.


A special thank to all the people that contributed to RGB development over the years, starting from Giacomo Zucco and Peter Todd who worked on the original client-side validated design back in 2017, to Alekos Filini who built the first prototype in 2018, to Maxim Orlovsky who led the development since 2019 and to the long list of all the developers who provided precious contributions in various ways.

This could be the start of a new chapter for RGB and everyone’s invited to clone the git repositories, install the rust crates from crates.io and check out the documentation at rgb.info. 

Thanks for being part of this journey.
