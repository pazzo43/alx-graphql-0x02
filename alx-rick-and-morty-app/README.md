# alx-graphql-0x02
## Duplicate Project
cp -R alx-graphql-0x01 alx-graphql-0x02

### Change Directory Into the App
cd alx-graphql-0x02/alx-rick-and-morty-app

#### Create Interfaces Directory
Inside the root of the project:
mkdir interfaces

Inside it:
touch interfaces/index.ts

##### Add TypeScript Interfaces
Open this file:
interfaces/index.ts

Paste exactly:
interface InfoProps {
    pages: number
    next: number
    prev: number
    count: number
}

export interface EpisodeProps {
  id: number
  name: string
  air_date: string
  episode: string
  info: InfoProps
}

export type EpisodeCardProps = Pick<EpisodeProps, 'id' | 'name' | 'air_date' | 'episode'>

##### Update pages/index.tsx
Open:
pages/index.tsx

Replace all content with:
import { useQuery } from "@apollo/client"
import { GET_EPISODES } from "@/graphql/queries"
import { EpisodeProps } from "@/interfaces"
import EpisodeCard from "@/components/common/EpisodeCard"
import { useEffect, useState } from "react"

const Home: React.FC = () => {

  const [page, setPage] = useState<number>(1)
  const { loading, error, data, refetch } = useQuery(GET_EPISODES, {
    variables: {
      page: page
    }
  })

  useEffect(() => {
    refetch()
  }, [page, refetch])

  if (loading) return <h1>Loading...</h1>
  if (error) return <h1>Error</h1>

  const results = data?.episodes.results
  const info = data?.episodes.info

  return (
    <div className="min-h-screen flex flex-col bg-gradient-to-b from-[#A3D5E0] to-[#F4F4F4] text-gray-800">
      {/* Header */}
      <header className="bg-[#4CA1AF] text-white py-6 text-center shadow-md">
        <h1 className="text-4xl font-bold tracking-wide">Rick and Morty Episodes</h1>
        <p className="mt-2 text-lg italic">Explore the multiverse of adventures!</p>
      </header>

      {/* Main Content */}
      <main className="flex-grow p-6">
        <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
          {results && results.map(({ id, name, air_date, episode }: EpisodeProps, key: number) => (
            <EpisodeCard
              id={id}
              name={name}
              air_date={air_date}
              episode={episode}
              key={key}
            />
          ))}
        </div>

        {/* Pagination Buttons */}
        <div className="flex justify-between mt-6">
          <button 
            onClick={() => setPage(prev => prev > 1 ? prev - 1 : 1)}
            className="bg-[#45B69C] text-white font-semibold py-2 px-6 rounded-lg shadow-lg hover:bg-[#3D9B80] transition duration-200 transform hover:scale-105">
            Previous
          </button>
          <button 
            onClick={() => setPage(prev => prev < info.pages ? prev + 1 : prev)}
            className="bg-[#45B69C] text-white font-semibold py-2 px-6 rounded-lg shadow-lg hover:bg-[#3D9B80] transition duration-200 transform hover:scale-105">
            Next
          </button>
        </div>
      </main>

      {/* Footer */}
      <footer className="bg-[#4CA1AF] text-white py-4 text-center shadow-md">
        <p>&copy; 2024 Rick and Morty Fan Page</p>
      </footer>
    </div>
  )
}

export default Home

##### Create EpisodeCard Component
Create the folders:
mkdir -p components/common
touch components/common/EpisodeCard.tsx

Paste this code:
import { EpisodeCardProps } from "@/interfaces";

const EpisodeCard = ({ id, name, air_date, episode }: EpisodeCardProps) => {
  return (
    <div key={id} className="bg-white cursor-pointer shadow-md rounded-lg p-4 m-4 transition-transform duration-200 hover:scale-105">
      <div className="flex justify-between items-center">
        <h2 className="text-xl font-semibold text-gray-800">{name}</h2>
        <span className="border px-2 text-xs rounded-full bg-blue-600 text-white flex items-center">{episode}</span>
      </div>
      <p className="text-gray-600">{air_date}</p>
    </div>
  );
};

export default EpisodeCard;

##### Run the App
npm run dev

Open:
http://localhost:3000


